# Raspberry Pi configuration and Bob setup

## Raspberry Pi OS installation

Download Raspberry OS Lite 64 bit

Mount it on the SD using the Raspberry Pi Imager which you can find at [this link](https://www.raspberrypi.com/software/):
- Select the Raspberry Pi OS Lite 64-bit
- Select the SD card
- Click on the settings button
- Specify username `pi` and password `raspberry`
- Specify the wifi with ssid `raspberryWiFi` and password `ciao1234`
- Enable SSH (should be enabled by default) with password
- Start the writing process

Activate the UART interface to be able to connect with the USB to UART connector
- Open `config.txt` in the SD `bootfs` partition
- Add at the end: `enable_uart=1`

Or, if you have a monitor and a keyboard:
- Open terminal and write `sudo raspi-config`
- Navigate to _Interface Options_, then _Serial Port_, choose _Yes_ for the login shell
- Reboot the Raspberry Pi
- Then follow [this guide](../debug-tools/raspberry_pi.md)
(if you have problems to find the device id try to see [this link](https://github.com/juliagoda/CH341SER/issues/18))

## Bob installation

Install Bob
- Copy Bob
  - Install git, vim, python3-dev
    ```
    sudo apt update
    sudo apt upgrade
    sudo apt install git vim python3-dev
    git --version
    ```
  - Clone Bob into `/home/pi/` (`~` folder) with HTTPS
    ```
    git clone https://github.com/policumbent/bob.git
    ```
- Install poetry
  - Install Rust
    ```
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    ```
  - Check that Rust has been correctly added to PATH
    ```
    rustc --version
    cargo --version
    ```
  - If not, add it manually to PATH and then check again
    ```
    source "$HOME/.cargo/env"
    ```
  - Install pip
    ```
    sudo apt install python3-pip
    pip --version
    ```
  - Install packets needed to install Poetry
    ```
    sudo apt-get install python3-distutils build-essential libssl-dev libffi-dev python-dev-is-python3
    pip install Cmake
    ```
  - Install Poetry and add it to PATH
    ```
    curl -sSL https://install.python-poetry.org | python3 -
    export PATH="/home/pi/.local/bin:$PATH"
    poetry --version
    ```

#### ANT setup

Setup ANT
- Add a new udev rule s.t. also non-root user can access the device
  ```
  cd /etc/udev/rules.d/
  sudo touch 99-garmin.rules
  ```
  then the file content should be
  ```
  SUBSYSTEM=="usb", ATTR{idVendor}=="0fcf", ATTR{idProduct}=="1008", MODE="666"
  ```
- Restart `udev` (or simply reboot the system) and reconnect the USB device

## FFS installation

- Clone Bob into `/home/pi/` (`~` folder) with HTTPS
  ```
  git clone https://github.com/policumbent/ffs.git
  ```

### Modules' setup

#### CAN setup

Setup CAN ([reference link](https://github.com/policumbent/poliCANbent/tree/main/rpi-can-config))
- Clone `poliCANbent` repository with HTTPS
  ```
  git clone https://github.com/policumbent/poliCANbent.git
  ```
- Go into `/poliCANbent/rpi-can-config/` and run the setup script
  ```
  chmod +x setup.sh
  ./setup.sh
  ```
- If the `cat` and append does not work, use `vim` and do copy and paste (for command number 2 and number 4 in `setup.sh`)
- It's possible to cancel the `poliCANbent` folder
- Since `can_logger.sh` is called from a Python program, run the following command to give it the permissions to do it
  ```
  chmod +x ~/bob/modules/can/can_logger.sh
  ```
- Reboot the system
- Run `ifconfig` and check that `can0` is present as network interface

#### Video setup

Setup PiCamera2 and video
- Install libcamera and picamera2
  ```
  sudo apt install -y python3-libcamera
  sudo apt install -y python3-picamera2 --no-install-recommends
  ```
- Run the following command to avoid `ImportError: libopenjp2.so.7: cannot open shared object file`
  ```
  sudo apt-get install libopenjp2-7
  ```
- Reboot
- Force the Raspberry to always output to HDMI (even if screen is booted before
or after the Raspberry) by uncomment in `/boot/firmware/config.txt` and modify
it by uncommenting the following line
  ```
  #hdmi_force_hotplug=1
  ```

#### Create `venv`

Go into `/home/pi/ffs/modules/video`, then run:
```bash
python -m venv ./venv --system-site-packages
source ./venv/bin/activate
pip install -r requirements.txt
deactivate
```

Then go to `/home/pi/ffs/modules/can`, then, similarly, run:
```bash
python -m venv ./venv
source ./venv/bin/activate
pip install -r requirements.txt
deactivate
```

#### Setup FFS/Bob services

- Go into `/home/pi/bob/modules`, then go in each module folder and copy the `.service` file into `/etc/systemd/system/`
  ```
  sudo cp <module>.service /etc/systemd/system/
  ```
- Change mode and enable each service
  ```
  sudo systemctl daemon-reload
  sudo systemctl enable <module>.service
  ```
- Reboot the system
- To see the status of the service, use the following command
  ```
  sudo systemctl status <module>.service
  ```

#### Troubleshooting
- To check why a module is failing, run manually the service by seeing the command specified in the `ExecStart` field of the `<module>.service` file contained inside `/home/pi/bob/modules/<module>/`, in general the command should be one of the following two
  1. For Poetry modules (all except video and CAN)
    ```
    /home/pi/bob/modules/<module>/.venv/bin/python3 -m src.main
    ```
  2. For non Poetry modules (video and CAN)
    ```
    cd /home/pi/bob/modules/<module>
    /usr/bin/python3 -m src.main
    ```
  - Video module
    - If you get a `numpy` error try to do
      ```
      sudo apt-get install libatlas-base-dev
      pip3 uninstall numpy
      sudo apt install python3-numpy
      ```
  - Ant module
    - If you get an error regarding a non existing `csv` file, simply create the folder
      ```
      cd ~/bob/
      mkdir csv
      ```
- Remember to add the name of the bike in the database `~/bob/database.db` into the table `configuration` under the primary key `global` writing inside the json file at the key `name` (for simplicity use [DB Browser for SQL Lite](https://sqlitebrowser.org/), s.t. you can see and modify the database without the need to make SQL queries)