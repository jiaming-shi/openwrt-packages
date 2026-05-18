# openwrt-packages
Self maintained packages for OpenWrt.

## How to Use

1. Add the feed to /etc/apk/repositories.d/customfeeds.list

    Edit (or create) the file /etc/apk/repositories.d/customfeeds.list and add:
    ```
    https://downloads.jiaming.sh/releases/25.12.4/packages/x86_64/custom/packages.adb
    ```

2. Import the feed signing key

    ```
    -----BEGIN PUBLIC KEY-----
    MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEKM468o2WJtZkiy7sI0mryNzI3gW8
    Gu975KBAJtEKBlToA9lIhbFq1fqI7HaUoJrCNbkY6T65Bdrfg80cy+zVBw==
    -----END PUBLIC KEY-----
    ```

    ```shell
    wget -O custom.pem https://downloads.jiaming.sh/keyring/apk/custom.pem
    mv custom.pem /etc/apk/keys/
    ```

3. Install packages from the feed

    After adding the feed and key, run:
    ```shell
    apk update
    apk add dae
    ```

## [Including the feed into OpenWrt build system](https://openwrt.org/docs/guide-developer/helloworld/chapter4)

1. Add the feed to feeds.conf.default

    Edit the feeds.conf.default file to download the needed package definitions.

    ```shell
    cd openwrt
    echo "src-git custom https://github.com/jiaming-shi/openwrt-packages.git;openwrt-25.12" >> feeds.conf.defaults
    ```

2. Updating and installing feeds

    ```shell
    ./scripts/feeds update custom
    ./scripts/feeds install -a -p custom
    ```

3. Compile packages

    ```shell
    # Build single package
    make -j$(nproc) package/dae/compile
    # Build all packages in this feed
    make -j$(nproc) package/compile
    
    make -j$(nproc) package/index CONFIG_SIGNED_PACKAGES=
    ```
