Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 4
      vb.customize ["modifyvm", :id, "--usb", "on"]
      vb.customize ["modifyvm", :id, "--usbehci", "on"]
      vb.customize ["usbfilter", "add", "0",
        "--target", :id,
        "--name", "ESP",
        "--manufacturer", "Silicon Labs",
        "--product", "CP2102 USB to UART Bridge Controller"]
  end

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    apt-get update

    sudo apt-get -y install make unrar-free autoconf automake libtool gcc g++ gperf \
    flex bison texinfo gawk ncurses-dev libexpat-dev python-dev python python-serial \
    sed git unzip bash help2man wget bzip2 linux-image-extra-virtual

    cd ~/
    mkdir -p build
    cd build
    git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
    cd esp-open-sdk
    make STANDALONE=y

    cd ../..

    echo 'export PATH=/home/vagrant/build/esp-open-sdk/xtensa-lx106-elf/bin:$PATH' >> .bashrc

    # Set the ESP USB port and make sure the vagrant user can write to it
    echo 'export ESPPORT=/dev/ttyUSB0' >> .bashrc
    sudo usermod -a -G dialout vagrant

    # Download esp-open-rtos
    git clone --recursive https://github.com/Superhouse/esp-open-rtos.git
    echo 'export SDK_PATH=/home/vagrant/esp-open-rtos' >> .bashrc

    git clone --recursive https://github.com/maximkulkin/esp-homekit-demo.git

    sudo modprobe usbserial
    sudo modprobe cp210x
  SHELL
end
