#Setup under Linux
## ~/.android/adb_usb.ini  
    # ANDROID 3RD PARTY USB VENDOR ID LIST -- DO NOT EDIT.
    # USE 'android update adb' TO GENERATE.
    # 1 USB VENDOR ID PER LINE.
    0x2717
## /etc/udev/rules.d/50-Android.rules 
    UBSYSTEM=="usb", ATTR{idVendor}=="2717", ATTRS{idProduct}=="0368", MODE="0666"
