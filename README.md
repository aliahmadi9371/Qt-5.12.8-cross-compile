# Qt-5.12.8-cross-compile

Host: 
  No LSB modules are available.
  Distributor ID:	Ubuntu
  Description:	Ubuntu 20.04.1 LTS
  Release:	20.04
  Codename:	focal

Target:
  Armbian 21.08.6 Focal with Linux 5.10.60-sunxi on nanopidue2
  
  
on Target:
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get install build-essential
  sudo apt-get install:
    qt5-default
    qt5-doc
    qt5serialport-examples
    qtattributionsscanner-qt5
    qtbase5-doc-html
    qtbase5-examples
    qtbase5-private-dev
    qtcreator
    qtmultimedia5-dev
    qtmultimedia5-doc
    qtmultimedia5-doc-html
    qtmultimedia5-examples
    qtquickcontrols2-5-dev
    qtquickcontrols2-5-doc
    qtquickcontrols2-5-doc-html
    qtquickcontrols2-5-examples
    qtserialport5-doc
    qtserialport5-doc-html
  
  sudo mkdir /usr/local/qt5pi
  sudo chown <user>:<user> /usr/local/qt5pi
  
  mkdir /opt/vc
  
on Host:
  download and install Qt 5.8.12 

  mkdir ~/<sdk-folder-name>
  cd ~/<sdk-folder-name>
  git clone https://github.com/raspberrypi/tools
  
  mkdir sysroot sysroot/usr sysroot/opt
  
  rsync -avz <user>@<target_ip>:/lib sysroot
  rsync -avz <user>@<target_ip>:/usr/include sysroot/usr
  rsync -avz <user>@<target_ip>:/usr/lib sysroot/usr
  rsync -avz <user>@<target_ip>:/opt/vc sysroot/opt
  
  wget https://raw.githubusercontent.com/riscv/riscv-poky/master/scripts/sysroot-relativelinks.py
  chmod +x sysroot-relativelinks.py
  ./sysroot-relativelinks.py sysroot
  
  download and install
  $ arm-linux-gnueabihf-g++ (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0
  Copyright (C) 2019 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

  $ arm-linux-gnueabihf-gcc --version
  arm-linux-gnueabihf-gcc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0
  Copyright (C) 2019 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
  
  
  run this command:
    wget http://download.qt.io/official_releases/qt/5.12/5.12.8/single/ qt-everywhere-src-5.12.8.tar.xz
    tar xvf  qt-everywhere-src-5.12.58.tar.xz
    cd  qt-everywhere-src-5.12.8
    
   important: (copy linux-sunxi-g++ folder from (/usr/lib//arm-linux-gnueabihf/qt5/mkspecs) on Targt to qt-everywhere-src-5.12.8/qtbase/mkspecs/devices on Host)
  
  run this command:
    -release -no-opengl -device linux-sunxi-g++ -device-option CROSS_COMPILE=/usr/bin/arm-linux-gnueabihf- -sysroot <path>/sysroot -opensource -confirm-license -skip qtwayland -skip qtlocation -skip qtscript -make libs -prefix /usr/local/qt5pi -extprefix <path>/qt5pi -hostprefix <path>/qt5 -no-use-gold-linker -v -no-gbm
    
    
  run this command:
    make (-j4)
    make install
    
  run this command:
    rsync -avz qt5pi <user>@<target_ip>:/usr/local
    
  add kit in qtcreator and use qt 5.12.8 cross-compile
