Note: Check readme-dev.md for develop branch

# Install Chipwhisperer software from github on Ubuntu
Uses a fresh Ubuntu system (tested on 16.04.1 LTE) and based on install instructions from : https://wiki.newae.com/Installing_ChipWhisperer

Supporting libraries:

``` 
$ sudo apt-get install python2.7 python2.7-dev python2.7-libs python-numpy python-scipy python-pyside python-configobj python-setuptools python-pip
$ sudo su
# pip install --upgrade pip ; exit
$ sudo -h pip install pyusb
$ sudo -H pip install pyqtgraph
```


Clone repo from github:
``` 
$ git clone https://github.com/newaetech/chipwhisperer
$ cd chipwhisperer/software
$ sudo python setup.py develop
```
You may also want the OpenADC software, which is necessary to build new firmware for the ChipWhisperer FPGA. This is unnecessary for most users. If you need it:
```
cd ..
git submodule init
git submodule update
cd openadc/controlsw/python
sudo python setup.py develop
```

AVR toolchain:
``` 
sudo apt-get install avr-libc gcc-avr
```

Hardware Drivers :

Edit/create the following file with the provided rules:
```
$ sudo vi /etc/udev/rules.d/99-newae.rules
```
```
# CW-Lite
SUBSYSTEM=="usb", ATTRS{idVendor}=="2b3e", ATTRS{idProduct}=="ace2", MODE="0664", GROUP="plugdev"

# CW-1200
SUBSYSTEM=="usb", ATTRS{idVendor}=="2b3e", ATTRS{idProduct}=="ace3", MODE="0664", GROUP="plugdev"

# CW-305 (Artix Target)
SUBSYSTEM=="usb", ATTRS{idVendor}=="2b3e", ATTRS{idProduct}=="c305", MODE="0664", GROUP="plugdev"

# CW-CR2
SUBSYSTEM=="usb", ATTRS{idVendor}=="04b4", ATTRS{idProduct}=="8613", MODE="0664", GROUP="plugdev"
SUBSYSTEM=="usb", ATTRS{idVendor}=="221a", ATTRS{idProduct}=="0100", MODE="0664", GROUP="plugdev"
```

Then add your username to the plugdev group:
```
$ sudo usermod -a -G plugdev YOUR-USERNAME
```

And reset the udev system:
```
$ sudo udevadm control --reload-rules
```

Finally log out & in again for the group change to take effect.



