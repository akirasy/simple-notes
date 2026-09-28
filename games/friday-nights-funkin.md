---
title  : Friday Nights Funkin
layout : default
parent : Games
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## Bundle game library

1. Create debian distrobox

2. Run this script: `bundle-geme.sh {executable}`

```
#!/usr/bin/env bash
set -euo pipefail

if [ "$#" -ne 1 ]; then
    echo "Usage: $0 /path/to/executable"
    exit 1
fi

EXE_PATH="$1"

if [ ! -f "$EXE_PATH" ] || [ ! -x "$EXE_PATH" ]; then
    echo "Error: Executable '$EXE_PATH' not found or not executable."
    exit 1
fi

EXE_NAME=$(basename "$EXE_PATH")
TARGET_DIR=$(dirname "$(realpath "$EXE_PATH")")
LIB_DIR="$TARGET_DIR/lib"
PLUGINS_DIR="$TARGET_DIR/plugins"

# Exclude base runtime/system libs (libvlc, libvlccore, libgamemode are NOT excluded)
EXCLUDE_REGEX="libc\.so|libm\.so|libdl\.so|libpthread\.so|librt\.so|ld-linux|libstdc\+\+|libgcc_s|libsystemd|libudev|libGL|libEGL|libdrm|libvulkan|libX11|libxcb|libwayland"

mkdir -p "$LIB_DIR"

echo "Resolving dynamic libraries for $EXE_NAME..."

# 1. Resolve direct dependencies from executable
LIBS_TO_COPY=$(ldd "$EXE_PATH" | awk '{print $3}' | sort -u | grep '^/' || true)

# 2. Force inclusion of known dlopen targets (VLC, GameMode)
for runtime_lib in libvlc.so libvlccore.so libgamemode.so; do
    SYS_LIB=$(ldconfig -p | grep "$runtime_lib" | awk '{print $NF}' | head -n 1 || true)
    if [ -n "$SYS_LIB" ]; then
        LIBS_TO_COPY=$(printf "%s\n%s" "$LIBS_TO_COPY" "$SYS_LIB")
    fi
done

# Copy resolved libraries and recursively copy their sub-dependencies
echo "$LIBS_TO_COPY" | sort -u | while read -r lib; do
    [ -z "$lib" ] && continue
    if echo "$lib" | grep -qvE "$EXCLUDE_REGEX"; then
        echo " -> Copying $lib"
        cp -L "$lib" "$LIB_DIR/" 2>/dev/null || true
        
        # Pull sub-dependencies for runtime libraries like libvlc
        ldd "$lib" 2>/dev/null | awk '{print $3}' | grep '^/' | while read -r sublib; do
            if echo "$sublib" | grep -qvE "$EXCLUDE_REGEX"; then
                cp -L "$sublib" "$LIB_DIR/" 2>/dev/null || true
            fi
        done
    fi
done

# 3. Detect and copy VLC plugins if libvlc is bundled
if [ -f "$LIB_DIR/libvlc.so"* ] || [ -f "$LIB_DIR/libvlc.so.5"* ]; then
    echo "Bundling VLC plugins..."
    mkdir -p "$PLUGINS_DIR"
    
    VLC_PLUGIN_SEARCH_PATHS=(
        "/usr/lib64/vlc/plugins"
        "/usr/lib/vlc/plugins"
        "/usr/lib/x86_64-linux-gnu/vlc/plugins"
        "/usr/local/lib/vlc/plugins"
    )

    VLC_FOUND=0
    for path in "${VLC_PLUGIN_SEARCH_PATHS[@]}"; do
        if [ -d "$path" ]; then
            echo " -> Copying VLC plugins from $path"
            cp -r "$path"/* "$PLUGINS_DIR/" 2>/dev/null || true
            VLC_FOUND=1
            break
        fi
    done

    if [ "$VLC_FOUND" -eq 0 ]; then
        echo "Warning: libvlc was detected, but no system VLC plugin directory was found!"
    fi
fi

# Create start.sh wrapper with VLC_PLUGIN_PATH exported
START_SCRIPT="$TARGET_DIR/start.sh"

cat << EOF > "$START_SCRIPT"
#!/bin/sh
SCRIPT_DIR="\$(cd "\$(dirname "\$0")" && pwd)"
LIB_DIR="\$SCRIPT_DIR/lib"
PLUGINS_DIR="\$SCRIPT_DIR/plugins"
TARGET="\$SCRIPT_DIR/$EXE_NAME"

if [ ! -x "\$TARGET" ]; then
    echo "Error: Executable '\$TARGET' not found or not executable."
    exit 1
fi

export LD_LIBRARY_PATH="\$LIB_DIR:\${LD_LIBRARY_PATH:-}"

if [ -d "\$PLUGINS_DIR" ]; then
    export VLC_PLUGIN_PATH="\$PLUGINS_DIR"
fi

exec "\$TARGET" "\$@"
EOF

chmod +x "$START_SCRIPT"

echo "Done! Bundled libraries into: $LIB_DIR"
[ -d "$PLUGINS_DIR" ] && echo "Bundled VLC plugins into:   $PLUGINS_DIR"
echo "Created runner for '$EXE_NAME' at: $START_SCRIPT"
```
