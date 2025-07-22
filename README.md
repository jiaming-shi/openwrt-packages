# openwrt-packages
Self maintained packages for OpenWrt.

## How to Use

1. Add the feed to /etc/opkg/customfeeds.conf

    Edit (or create) the file /etc/opkg/customfeeds.conf and add:
    ```
    src/gz custom_packages https://downloads.jiaming.sh/releases/packages-24.10/x86_64/custom
    ```

2. Import the feed signing key

    ```
    RWTtvFBjE+kD6fi/0Dnb7A839xoZp0eR/2wzLtn+1vzUUY229dG1sFVC
    ```

    ```shell
    wget -O usign.pub https://downloads.jiaming.sh/keyring/usign/edbc506313e903e9
    opkg-key add usign.pub
    ```

3. Install packages from the feed

    After adding the feed and key, run:
    ```shell
    opkg update
    opkg install dae
    ```
