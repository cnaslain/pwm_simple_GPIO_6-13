# Raspberry Pi simple PWM 4 pins fan solution

This is based on https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=244194&start=25#p1511085 article post by Artain; thanks to him for this simple PWM solution.

## Hardware and building
- Raspberry Pi 4 B (but it should work on all 40 pins GPIO Pi boards)
- Noctua NF-A4x10 5V PWM 4 pins cooler
- A 2.2 kΩ resistance

Connexion of the 4 pin wire:
- Black wire: GND
- Yellow wire: 5V
- Green: Tacho on GPIO 6
- Blue: PWM on GPIO 13

Wire chart: ![Wiring diagram](electronic_diagram.jpg)

## Software
I've just modified the following stuffs on the python script:
- default path to the rpm file
- new log file for the data output, so I can read them with a LUA script in Domoticz

I've also added a new simple systemd service.

Setup:
```
cd /home/pi
git clone <THIS REPO> pwm_fan
cd pwm_fan
chmod 755 /home/pi/pwm_fan/pwm_simple_GPIO_6-13.service
sudo ln -s /home/pi/pwm_fan/pwm_simple_GPIO_6-13.service /etc/systemd/system/pwm_simple_GPIO_6-13.service
sudo systemctl daemon-reload
sudo systemctl start pwm_simple_GPIO_6-13
sudo systemctl status -a pwm_simple_GPIO_6-13
sudo systemctl enable pwm_simple_GPIO_6-13
```

Example of status output:
```
pi@raspberrypi4:~/pwm_fan $ sudo systemctl status -a pwm_simple_GPIO_6-13
● pwm_simple_GPIO_6-13.service - Simple GPIO 613 PWM fan script
   Loaded: loaded (/home/pi/pwm_fan/pwm_simple_GPIO_6-13.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-01-16 22:00:59 CET; 42min ago
 Main PID: 12036 (python)
    Tasks: 3 (limit: 4915)
   Memory: 2.2M
   CGroup: /system.slice/pwm_simple_GPIO_6-13.service
           └─12036 /usr/bin/python /home/pi/pwm_fan/pwm_simple_GPIO_6-13.py

Jan 16 22:00:59 raspberrypi4.xxx.com systemd[1]: Started Simple GPIO 613 PWM fan script.
```

Logs:
```
pi@raspberrypi4:~/pwm_fan $ cat /var/log/pwm_simple_GPIO_6-13.rpm
1240

pi@raspberrypi4:~/pwm_fan $ cat /var/log/pwm_simple_GPIO_6-13.log
actualTemp 46.74 TempDiff 1.74 pDiff 8.69 pwmDuty    17
```
