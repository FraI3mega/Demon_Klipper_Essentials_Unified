# MAINLINE YOUR MAX! 

Use the Sovol image but convert it to mainline Klipper! NO STLINK REQUIRED!

<br>

# :gift: SUPPORT YOUR FRIENDLY 3DPrintDemon! :gift:

>[!TIP]
>Please consider supporting this project…. Even if it’s for a single donation!
>
>It really does make a difference & any amount you send is greatly appreciated!

This macro pack is the cumulation of 3 years of work by one person alone, there have been countless late nights, missed family time, bottomless cups of coffee, as well as a boat load of effort & dedication. There’s been endless weeks of writing code & then probably that amount of time again at least thoroughly testing the files so that you can rest assured that they work & these macros will NOT damage or harm your printer in any way! Not counting any improper setup of course…

Plus I provide the DEMON DISCORD to help anyone with getting DKEU working on their system!

All of this is given away to the community for FREE.

However I would like to kindly ask that if you gain any sort of benefit, joy, improved quality of life using your printer, or maybe if you use these macros as part of your side or regular business, for example in your print farm or to help make your craft fair items please consider making a pledge on the 3DPrintDemon Patreon page for however much you feel these macros are worth to your printing life & your business income! You can choose the amount of your donation & how long you are an active donating supporter!

You can stay a supporter on the 3DPrintDemon Patreon sending donations of your choosing for as long as you like. Maybe it’s for just a month or two for single private users that would like to show your gratitude, or maybe you could consider ongoing support if you’re a business owner & make regular use of my work to aid your business.

Active supporters have a special channel on the Demon Discord server & are provided with a higher level of support over non supporting members. Just make your discord user name known & you’ll be granted “supporter” privileges.

:red_circle: [JOIN THE 3DPRINTDEMON PATREON](https://patreon.com/3dprintdemon) as an active donating member & show your support for my work!

Your donations are used to feed my printers & give them the latest fancy pants new parts so I can continue adding new features, fixing bugs & providing time for helping you all to get the macros running & fixing issues you might experience!

Be sure to use the website not the IOS app, it's cheaper!

<br>
<br>

# WARNING! WITH ANY VERSION OF THE LATEST KLIPPER YOU WILL LOOSE THE ABILITY TO RUN THE BUFFER_STEPPER!!!!

# YES YOU READ CORRECTLY BY DOING THIS YOU WILL LOOSE YOUR AUX FEEDER!!! STOP NOW IF YOU DON'T WANT THAT!

This is due to fundamental changes in recent releases of Klipper that render the Sovol code incompatible!

<br>

## :warning: MAKE A BACKUP!

Make a backup of your current system now! Be sure you at least download your current `/config` folder BEFORE YOU DO ANYTHING ELSE!! You want have a set of UNTOUCHED files to refer back to if needed! Once a backup is saved on your computer you can use it to drop files back into this fresh install once you finish the guide! Also be sure to use a new SD card or SSD - this will leave your old system safe & completely untouched!

<br>

<br>

# :red_circle: FOLLOW THIS GUIDE

Make sure you do it ALL! Then come back here.

### [HOW TO FIX & UPDATE STOCK SOVOL EMMC IMAGES - CLICK HERE!](https://github.com/3DPrintDemon/Demon_Klipper_Essentials_Unified/blob/main/Documentation/INSTALL_INSTRUCTIONS/Fix_Sovol_EMMC_Images/FIX_SOVOL_EMMC_IMAGES.md)

<br>

# :red_circle: PREPARATION

SSH command...

```
cd kiauh && git pull
```
then 

```
./kiauh/kiauh.sh
```

The hit option `S` & check the first section for reads Klipper source repository reads 

`Repo: github.com/Klipper3d/klipper`

`Branch: master`

:red_circle: Once once confirmed navigate to option E (Community: Extensions), then option 1 (G-code Shell Command) to REMOVE & then INSTALL the extension over.

<br>

# :red_circle: DELETIONS

Press `3` to `Remove` software!

Here we want to COMPLETELY remove Sovol Klipper!

Press `1` to remove `Klipper`

<br>

# :red_circle: INSTALL NEW

Now back to the main menu `B`

Go to `1 Install`

the select `1 Klipper` & install new mainline Klipper!

Now back to the main menu `B`

hit `Q` to exit!

<br>

Next head over to [Eddy NG](https://github.com/vvuk/eddy-ng) & install their fine upgrade!

```
cd ~
git clone https://github.com/vvuk/eddy-ng
```

& run the installer...

```
cd ~/eddy-ng
./install.sh
```

Reboot now!

```
sudo reboot now
```

<br>

# NOW THE TRICKY BIT! 

At the time of writing we need to edit Klipper to let us use a 128kib bootloader for the mainboard MCU! To do this we need to......

```
sudo service klipper stop
```

>[!CAUTION]
>Be EXTREMELY careful with this file & do NOT edit any other edits or disrupt it's formatting in any way!

```
sudo nano ~/klipper/src/stm32/Kconfig
```

Scroll down to where it says `Bootloader` section surrounded by hashes in blue 

now add...

```
 || MACH_STM32H750
```

...to the end of the line that says `bool "128KiB bootloader" so it looks exactly the same as the rest of the section as per the image below.

<br>

<img width="895" height="576" alt="EDIT" src="https://github.com/user-attachments/assets/801e1a3d-1fab-42a1-a32f-d7b68d255568" />

<br>
<br>
<br>

Once you have confirmed this is done correctly press `ctrl+X` & press `Y` to save the file & the hit `return` to confirm the name - leave it unchanged!

```
sudo service klipper start
```

....& remember to breathe! Phew!

<br>

# :red_circle: BIG RED ERROR TIME!

We should now have a new mainline install of Klipper & it'll probably be giving you a big red error screen & some Moonraker warnings about depreciated MCU firmware. Don't worry this is totally normal & expected!

Its just telling you your current MCU firmware is out of date! We're gonna fix that in a minute!

<br>

# :red_circle: FLASHING NEW MCU FIRMWARE!

>[!IMPORTANT]
>The SV08 Max comes with KATAPULT already installed on ALL MCU's!! So all we need do is issue the jump command & update the firmware!

<br>

>[!WARNING]
>The MCU UUID's will change after the flash so be sure to use the command to search for them! See below!

Scan for UNASSIGNED Canbus UUID's - this will NOT show any assigned nodes!

<br>

```
~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```
<br>

Head over to here for updating your mainboard mcu
https://canbus.esoterical.online/mainboard_klipper_updating.html

#### SETTINGS FOR THE SOVOL SV08 MAX MAINBOARD:



<img width="615" alt="MAX MAIN MCU" src="https://github.com/user-attachments/assets/c0ace630-1174-4255-aaa3-4c040fde753b" />

<br>
<br>
<br>



Now heat to here to update the toolhead!
https://canbus.esoterical.online/toolhead_klipper_updating.html

#### SETTINGS FOR THE SV08 MAX EXTRA MCU & HOT MCU (toolhead & chamber heater!):

<img width="615" height="235" alt="MAX OTHER MCUS" src="https://github.com/user-attachments/assets/78ff3213-0774-4c6d-9c85-aa8d91cdef77" />

<br>
<br>
<br>


# :red_circle: BUILD HOST MCU FIRMWARE

```
cd ~/klipper/
sudo cp ./scripts/klipper-mcu.service /etc/systemd/system/
sudo systemctl enable klipper-mcu.service
```

```
make menuconfig
```

Set to compile firmware for `Linux process`

Then...

```
sudo service klipper stop
make flash
sudo service klipper start
```

<br>

Now add this expanded section below to your printer.cfg commenting out the Sovol one shown here.

```
# [probe_eddy_current eddy]
# sensor_type: ldc1612
# z_offset: 3.50
# i2c_mcu: extra_mcu
# i2c_bus: i2c2
# x_offset: -19.8
# y_offset: -0.75
# # vir_contact_speed: 3.0
```

```
[probe_eddy_ng eddy]
sensor_type: ldc1612
i2c_mcu: extra_mcu
i2c_software_scl_pin: extra_mcu:PB10
i2c_software_sda_pin: extra_mcu:PB11
# i2c_bus: i2c2
x_offset: -19.8
y_offset: -0.75

probe_speed: 5.0
lift_speed: 10.0
move_speed: 50.0
home_trigger_height: 2.0
calibration_z_max: 5.0
reg_drive_current: 15
tap_drive_current: 0
tap_start_z: 3.0
tap_target_z: -0.350
tap_mode: butter
tap_threshold: 250
tap_speed: 3.0
tap_adjust_z: 0.000
tap_samples: 3
tap_max_samples: 8
tap_samples_stddev: 0.025
```

Save & Restart

<br>

# :red_circle: CONFIGURE EDDY NG!

Follow the configuration steps here.
https://github.com/vvuk/eddy-ng/wiki/Calibration


# :red_circle: FINISH!

You should now be on Mainline Klipper running Eddy NG!

<img width="839" height="467" alt="DONE!" src="https://github.com/user-attachments/assets/97440e60-9aae-48f9-8906-53c40fc67309" />
