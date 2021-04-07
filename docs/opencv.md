# Installazione OpenCV

1. Abilitare la camera da `sudo raspi-config`
3. `sudo apt install python3-pip python3-picamera libatlas-base-dev libopenjp2-7 libilmbase-dev libopenexr-dev libgstreamer1.0-dev libavcodec-dev  libavformat-dev  libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libdc1394-22-dev libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev -y`
4. `pip3 install opencv-python imutils`

## Installazione GUI

1. `sudo apt update`
2. `sudo apt install jwm x11-apps`
3. `wget https://www.policumbent.it/config_files/.jwmrc`
4. `wget -O splash.png https://raw.githubusercontent.com/policumbent/Policumbent-ant-schermi/master/utils/splash.png?token=AKTACIFYJVF4JVFSCRZNTSDABQHVO`
5. `sudo apt install xorg`
6. `sudo apt install sakura`
7. `/home/pi/.bash_profile`
8. Incollare 
```
if [ -z $DISPLAY ] && [ $(tty) = /dev/tty1 ]
then
  startx
fi
```
9. `sudo reboot` (`startx` per avviare senza riavviare)

Forse c'è da aggiungere

1. `sudo raspi-config`
2. `System Options`
3. `Boot / Auto Login`
4. `Console Autologin`

## Impostazione risoluzione schermi

```txt
# uncomment to force a specific HDMI mode (here we are forcing 800x480!)
hdmi_group=2
hdmi_mode=87
hdmi_cvt=800 480 60 6 0 0 0
hdmi_drive=1
```
