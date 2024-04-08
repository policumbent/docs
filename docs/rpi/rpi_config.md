# Raspberry Pi configuration and Bob setup

## Raspberry Pi OS installation

Download Raspberry OS Lite 32 bit

Mount it on the SD using the Raspberry Pi Imager which you can find at [this link](https://www.raspberrypi.com/software/):
- Select the Raspberry Pi OS Lite 32-bit based on Debian Bullseye
- Select the SD card
- Click on the settings button
- Specify username `pi` and password `raspberry`
- Specify the wifi with ssid `raspberryWiFi` and password `ciao1234`
- Enable SSH (should be enabled by default) with password
- Start the writing process

Activate the UART interface to be able to connect with the USB to UART connector
- Open `config.txt` in the SD `bootfs` partition
- Add at the end: `enable_uart=1`

Or, if you have a monitor:
- Open terminal and write `sudo raspi-config`
- Navigate to _Interface Options_, then _Serial Port_, choose _Yes_ for the login shell
- Reboot the Raspberry Pi
- Then follow [this guide](../debug-tools/raspberry_pi.md)
(if you have problems to find the device id try to see [this link](https://github.com/juliagoda/CH341SER/issues/18))

## Bob installation

Install Bob
- Copy Bob
  - Install git
    ```
    sudo apt update
    sudo apt upgrade
    sudo apt install git
    git --version
    sudo apt install vim
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
  - Use _custom installation_ and specify for _Default host triple?_ enter `arm-unknown-linux-gnueabihf` (needed since it's a 32 bit installation, if you miss this step you will not be able to add Rust to PATH)
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
    sudo apt-get install python3-distutils build-essential libssl-dev libffi-dev python-dev
    pip install Cmake
    ```
  - Install Poetry and add it to PATH
    ```
    curl -sSL https://install.python-poetry.org | python3 -
    export PATH="/home/pi/.local/bin:$PATH"
    poetry --version
    ```
- Create the virtual environments for the Bob modules
  - Go into `/home/pi/bob/modules`
  - Enter in every module folder, except `CAN` and `Video`, and run
    ```
    poetry install
    ```
  - Instead `CAN` and `Video` folder and run
    ```
    pip3 install -r requirements.txt
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

#### Video setup

Setup PiCamera and video
- Enable the camera
  - Open terminal and write `sudo raspi-config`
  - Navigate to _Interface Options_, then _Legacy Camera_, choose _Yes_ for the camera enable
  - Reboot the Raspberry Pi
- Run the following command to avoid `ImportError: libopenjp2.so.7: cannot open shared object file`
  ```
  sudo apt-get install libopenjp2-7
  ```
- Force the Raspberry to always output to HDMI (even if screen is booted before or after the raspberry)
  - Take the SD card, from pc uncomment in `boot` search `config.txt` and modify it by uncommenting the following line
  ```
  #hdmi_force_hotplug=1
  ```

#### Setup Bob services

- Go into `/home/pi/bob/modules`, then go in each module folder and copy the `.service` file into `/etc/systemd/system/`
  ```
  sudo cp <module>.service /etc/systemd/system/
  ```
- Change mode and enable each service
  ```
  sudo systemctl daemon-reload
  sudo systemctl enable <module>.service
  ```
- Setup mosquitto broker
  - Install mosquitto
    ```
    sudo apt update && sudo apt upgrade
    sudo apt install mosquitto mosquitto-clients
    ```
  - Add mosquitto as a service to be automatically started at boot up
    ```
    sudo systemctl enable mosquitto.service
    ```
  - Test the installation
    ```
    mosquitto -v
    ```
    the output will be similar to something like this
    ```
    1692454743: mosquitto version 2.0.11 starting
    1692454743: Using default config.
    1692454743: Starting in local only mode. Connections will only be possible from clients running on this machine.
    1692454743: Create a configuration file which defines a listener to allow remote access.
    1692454743: For more details see https://mosquitto.org/documentation/authentication-methods/
    1692454743: Opening ipv4 listen socket on port 1883.
    1692454743: Error: Address already in use
    1692454743: Opening ipv6 listen socket on port 1883.
    1692454743: Error: Address already in use
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