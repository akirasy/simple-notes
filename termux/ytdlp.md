---
title  : YT-DLP
layout : default
parent : Android Termux
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Installation

Termux has prepackaged this software. Just run:
```
pkg update && \
pkg install -y python-yt-dlp
```

## Scripts

Easy script for downloading video and audio. Downloaded file will be placed in device `downloads` folder.

Just save this script (with a name of your liking) and put it in `$PATH` directory. Just remember to make it executable.

```
#!/bin/bash

echo ""
echo "         _            _ _       "
echo "   _   _| |_       __| | |_ __  "
echo "  | | | | __|____ / _  | | '_ \ "
echo "  | |_| | ||_____| (_| | | |_) |"
echo "   \__, |\__|     \__,_|_| .__/ "
echo "   |___/                 |_|    "
echo ""

# Set variables
FILENAMETEMPLATE="%(title)s.%(ext)s"
OUTPUTFOLDER="/data/data/com.termux/files/home/storage/downloads"

# Check if the file is writable
if ! [ -w "$OUTPUTFOLDER" ]; then
  echo "The output folder is not writable."
  echo "Please check your termux-setup-storage permission."
  exit 1
fi

# Check if yt-dlp is installed
command -v yt-dlp >/dev/null 2>&1
if [ $? -ne 0 ]; then
  echo "======== yt-dlp ============"
  echo "-- Installing yt-dlp..."
  echo "-- Please wait..."
  pkg update >/dev/null 2>&1 && pkg install -y python-yt-dlp >/dev/null 2>&1; 
  echo "-- yt-dlp installed successfully!"
  echo ""
fi

# Check if termux-api is installed
command -v termux-clipboard-get >/dev/null 2>&1
if [ $? -ne 0 ]; then
  echo "======== termux-api ========"
  echo "-- Installing termux-api..."
  echo "-- Please wait..."
  pkg update >/dev/null 2>&1 && pkg install -y termux-api >/dev/null 2>&1; 
  echo "-- termux-api installed successfully!"
  echo ""
fi

# Verify copied clipboard
echo "Copied link:"
echo "--> $(termux-clipboard-get)"
echo ""
echo "Is this correct?"

# Get user input and begin download
echo ""
echo "If yes, what do you want to do?"
echo "1) Download video"
echo "2) Download audio only"
echo "*) Enter any key to quit"
echo ""

read -p "Enter your choice: " INPUT
case $INPUT in
  1)
    yt-dlp --path $OUTPUTFOLDER --output $FILENAMETEMPLATE --format "mp4" $(termux-clipboard-get)
    ;;
  2)
    yt-dlp --path $OUTPUTFOLDER --output $FILENAMETEMPLATE --format "m4a" $(termux-clipboard-get)
    ;;
  *)
    echo "Script aborted."
    ;;
esac

echo ""
```

