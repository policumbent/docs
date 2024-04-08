# Raspberry Pi

Since the Raspberry Pi cannot be connected to "standard peripherals", in order
to work with it we have two options: use SSH or the UART.

## SSH

In order to connect to SSH, you will first have to edit the
``/etc/wpa_supplicant/wpa_supplicant.txt`` appending the internet access point
to the file as follows:

```
network={
    ssid="YOUR_SSID"
    psk="YOUR_PASSWORD"
}
```

You can do that just puttin the Raspberry's SD Card in your PC and open the
``rootfs`` partition.

*Be careful!* The ``rootfs`` partition cannot be opened on
Windows, since it does not recognize Linux filesystems.

You can also modify the ``wpa_supplicant.conf`` via [UART](#uart).

You will need to connect your computer on the same LAN of the Raspberry (e.g.:
via an hotspot) and you will need to know the Raspberry IP address. In order to
know that, you can connect via UART and run ``ifconfig``.

When you know the IP address, you can then run, from your computer's terminal:
```bash
ssh pi@<ip_address>
```

## UART

The Raspberry Pi offers the possibility of connecting to its UART, which
corresponds to a TTY session, so, using a serial communication tool, you can
access to a session of Raspberry OS. You can connect to the UART pins using a
TTL USB-UART converter. Being UART, you have to remember that RX and TX pins are
switched in the devices.

| RPi Pin | RPi Label        | USB-UART converter |
|:-------:|:----------------:|:------------------:|
| 6       | GND              | GND                |
| 8       | GPIO14 (UART TX) | RX                 |
| 10      | GPIO15 (UART RX) | TX                 |

You can use [this website](https://pinout.xyz/#) to have a better understanding
of the Raspberry's pinout.

Once the TTL USB-UART converter is correctly connected, you will have to open a
serial communication program. I advise using [``tio``](https://github.com/tio/tio),
which is pretty straightforward to use and works well (another option can be
``minicom``, but it's probably more complex to use, so it won't be reported
here).

### Installing ``tio``

- Ubuntu: ``snap install tio --classic``
- Arch Linux: ``yay -S tio`` (if you're using Arch and the AUR without using yay
you're probably smart enough to find a way to install it with your AUR helper :) )

### Using ``tio``

To launch ``tio``, you will have just to call: ``tio <your-device-position>``
and you're connected via UART to the Raspberry; at that point you can visualize
the Raspberry's TTY and use it as a normal terminal session.

If you cannot find the position of the UART device, you can run, before
re-inserting the converter, ``sudo dmesg -wH`` which will show the system log
so that when you will insert again the converter, ``dmesg`` will show its
position.

In order to close ``tio`` you will have to press ``<Ctrl + T>`` and then ``q``.

While you're connected to the Raspberry, you can modify its files (also the
``wpa_supplicant.conf``) and launch ``ifconfig`` to see its IP address (which
can be messy to find).