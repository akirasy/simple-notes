---
title  : Local Binhost
layout : default
parent : Gentoo Linux
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Gentoo offers prebuilt binary packages through the Gentoo binary package host. And we could build our own too. Packages can be installed without the need to compile code locally, or to install build-time dependencies!

## General rule

For binary packages made on one system to be usable on other systems they must fulfill some requirements:

- The builder and client architecture and CHOST must match.
- The CFLAGS and CXXFLAGS variables used to build the binary packages must be compatible with all clients.
- USE flags for processor specific instruction set features (like MMX, SSE, etc.) must be carefully selected; all clients must support them.

## Host

### Prepare `/etc/portage/make.conf`

1. Edit `/etc/portage/make.conf` to satisfy to following

    ```
    COMMON_FLAGS="-march=x86-64-v3 -O2 -pipe"
    ```

1. CPU Flags: Run this on the weakest CPU, then use the `CPU Flags` for binary host compiler

    ```
    emerge --ask --oneshot app-portage/cpuid2cpuflags
    echo "*/* $(cpuid2cpuflags)" >> /etc/portage/package.use
    ```

### Setup a Local GPG Signing Key

1. **Generate a dedicated GPG key with signing capabilities on your build host:**

    ```
    gpg --full-generate-key
    ```

    * Choose **`(1) RSA and RSA (default)`** or **`(4) RSA (sign only)`**.
    * Set key size to **`4096`** bits.
    * Set expiration as desired (e.g., `0` for no expiration).
    * Enter user details (e.g., *Local Binhost Key*).

1. **List  secret keys to obtain the full fingerprint:**

    ```
    gpg --list-secret-keys --keyid-format LONG
    ```

    _Example Output:_
    ```
    sec   rsa4096/EBAAC2474A0F97F5 2026-09-13 [SC]
          173AF510420F98DCBFF14C42EBAAC2474A0F97F5
    uid           [ultimate] localbinhost <local@binhost>
    sub   rsa4096/EE5FAACEDC774890 2026-09-13 [E]
    ```

    * **Short Key ID:** `EBAAC2474A0F97F5` (after `rsa4096/`)
    * **Full Key ID (Fingerprint):** `173AF510420F98DCBFF14C42EBAAC2474A0F97F5` (the 40-character string under `sec`)

    > **Note:** Always prefer using the **full 40-character fingerprint** (`173AF510420F98DCBFF14C42EBAAC2474A0F97F5`) for Portage configuration to ensure maximum security and avoid key ID collision.

1. **Configure `/etc/portage/make.conf`**

    ```
    # Set the signing key fingerprint
    BINPKG_GPG_SIGNING_KEY="173AF510420F98DCBFF14C42EBAAC2474A0F97F5"

    # Automatically build, sign, and enforce signature verification
    FEATURES="${FEATURES} buildpkg binpkg-signing binpkg-request-signature"
    ```

    * **`buildpkg`**: Creates binary package archives during `emerge`.
    * **`binpkg-signing`**: Signs new packages using `BINPKG_GPG_SIGNING_KEY`.
    * **`binpkg-request-signature`**: Requires valid GPG signatures before installing binary packages.

1. **Import the Public Key into Portage's Verification Keyring**

    1. Export the Public Key

    ```
    gpg --armor --export 173AF510420F98DCBFF14C42EBAAC2474A0F97F5 > /tmp/binpkg-public.asc
    ```

    1. Initialize Portage's Verification Directory

    ```
    getuto
    ```

    1. Import Public Key to Portage Keyring

    ```
    gpg --homedir=/etc/portage/gnupg --import /tmp/binpkg-public.asc
    ```

    1. Trust & Locally Sign the Key

    ```
    sudo gpg --homedir=/etc/portage/gnupg \
    --passphrase-file /etc/portage/gnupg/pass \
    --lsign-key 173AF510420F98DCBFF14C42EBAAC2474A0F97F5
    ```

    1. Clean Up Temporary File

    ```
    rm /tmp/binpkg-public.key
    ```

### Serving Binary Packages via Caddy (HTTP)

1. **Install Caddy**

    ```
    emerge --ask www-servers/caddy
    ```

1. **Configure Caddyfile**

    Edit `/etc/caddy/Caddyfile`:

    ```
    :8080 {
        root * /var/cache/binpkgs
        file_server browse
    }
    ```

1. **Start & Enable Caddy**

    ```
    systemctl start caddy
    ```



## Client

### Setup binary package repository

1. Set up the binary package repository in `/etc/portage/binrepos.conf`.

    ```
    [gentoo]
    priority = 1
    sync-uri = https://distfiles.gentoo.org/releases/amd64/binpackages/23.0/x86-64
    location = /var/cache/binhost/gentoo
    verify-signature = true

    [local-binhost]
    priority = 100
    sync-uri = http://{HOST-IP-DOMAIN-ADDRESS}:8080
    location = /var/cache/binhost/local-binhost
    verify-signature = true
    ```

1. Edit `/etc/portage/make.conf` to satisfy to following.

    ```
    FEATURES="${FEATURES} getbinpkg binpkg-request-signature"
    ```

### Import and Trust the Host's Public Signing Key

1. **Transfer the public key from host into the client PC**

    Use `scp` or any other method to transfer the file

1. **Initialize Portage's verification keyring on client**

    ```
    getuto
    ```

1. **Import host's public key**

    ```
    gpg --homedir=/etc/portage/gnupg --import binpkg-public.asc
    ```

1. **Trust and sign key locally using getuto password**

    ```
    gpg --homedir=/etc/portage/gnupg \
    --passphrase-file /etc/portage/gnupg/pass \
    --lsign-key 173AF510420F98DCBFF14C42EBAAC2474A0F97F5
    ```

1. **Clean up exported key file**

    ```
    rm binpkg-public.asc
    ```

1. Fix file permission

    ```
    sudo chmod -R 775 /etc/portage/gnupg
    sudo find /etc/portage/gnupg -type f -exec chmod 664 {} +
    ```

    > If not done, `gpg` will produce errors as below
    ```
    gpg: failed to create temporary file 'etc/portage/gnupg/.#1k0x....' : Permission denied
    ```

### Client PC setup completed. Enjoy!

1. **Install binary packages using Portage as usual**

    ```
    emerge -av <category/package-name>
    ```
