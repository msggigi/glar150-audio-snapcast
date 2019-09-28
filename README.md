# glar150-audio-snapcast
Instructions how to install drivers for USB soundcard on GL-inet GL-AR150 router and play multiroom audio with it

HW requirements:
- GL-inet AR-150 router (I used Openwrt firmware version 18.06.1)
- supported USB sound stick (I got mine from berrybase.de)
- device running the snapcast server (for testing purposes you can use your smartphone with snapcast app)

Follow these steps to get your sound up and running on a GL-AR-150:

1.  Log in your router with ssh.

1.  Make sure your package lists are up-to-date.
    ```
    opkg update
    ```

2.  Install the packages for USB (see https://openwrt.org/docs/guide-user/hardware/audio/usb.audio).
    ```
    opkg install kmod-usb-audio
    ```
    This also installs dependent package "kmod-sound-core". USB sound card should be detected when plugged in.
    ```
    root@GL-AR150:~# lsusb
    Bus 001 Device 002: ID 0d8c:0014 C-Media Electronics, Inc. Audio Adapter (Unitek Y-247A)
    ...
    ```
    If "lsusb" is not found, you can install with "opkg install usbutils".

3.  Install the packages for sound (see https://openwrt.org/docs/guide-user/hardware/audio/pulseaudio).
    ```
    opkg install alsa-lib libsndfile alsa-utils
    ```
    When you run "alsamixer" now, you should see the USB device and should be able to control the volume of it.
    
    You can test sound by streaming internet radio station to madplay.
    ```
    wget -q -O- http://br-br1-mainfranken.cast.addradio.de/br/br1/mainfranken/mp3/56/stream.mp3 | madplay - 
    ```
4. Download the most recent release of snapcast client to your router (from https://github.com/badaix/snapcast/releases/) and install it.
   If you don't have SSL support installed on your router, it is easiest to download it on your local machine and transfer it with scp.
    ```
    echo On host computer: for latest release of snapcast at the time of writing this tutorial
    wget https://github.com/badaix/snapcast/releases/download/v0.15.0/snapclient_0.15.0_mips_24kc.ipk
    scp snapclient_0.15.0_mips_24kc.ipk root@YOURROUTERIPADDRESS:
    ```
    On the router install it with:
    ```
    opkg install snapclient_0.15.0_mips_24kc.ipk
    ```


Resources:
- http://download.gl-inet.com/releases/packages-3.x/ar71xx/packages/
- https://openwrt.org/docs/guide-user/hardware/audio/usb.audio
- https://openwrt.org/docs/guide-user/hardware/audio/pulseaudio
- https://github.com/badaix/snapcast/


