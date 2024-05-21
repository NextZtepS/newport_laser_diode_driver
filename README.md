# Newport Model 500B Series Laser Diode Driver USB Package (for Linux)


## Pre-requisites

- Permission to connect to the Newport Model500B USB device
- Python version >= 3
- pyusb library


## Set-up

### Allow the Newport Model500B USB device to be managed by Python [ref](https://stackoverflow.com/questions/31992058/how-can-i-comunicate-with-this-device-using-pyusb/31994168#31994168)

Run the following shell command:

```shell
cd /lib/udev/rules.d/ ;
sudo touch 50-Newport500Busb.rules ;
sudo vim 50-Newport500Busb.rules ;
```

Copy & paste the following line into the file: `ACTION=="add", SUBSYSTEMS=="usb", ATTRS{idVendor}=="104d", ATTRS{idProduct}=="1001", MODE="660", GROUP="plugdev"`

Then, type `:wq` and press `ENTER` to save the changes

Run the following shell command:

```shell
sudo adduser USERNAME plugdev ;  # don't forget to change the USERNAME
sudo udevadm control --reload ;
sudo udevadm trigger ;
```

### Prepare Python and dependencies

In your desired Python environment, run the following shell command:

```shell
pip install pyusb  # should be already installed by default
```


## Usage

Here is an example snippet of how a Laser Driver object could be instantiated:

```python
model_535B = NewportLaserDriver(idVendor=0x104d, idProduct=0x1001)

identifier = model_535B.get_identification()  # obtain the identification
print(identifier)

model_535B.set_current_set_point(10.0)  # set current set point to be 10.0 mA
current = model_535B.get_current_set_point()  # get the current set point
print(current)  # 10.0
model_535B.enable_laser_output()  # enable laser output
```

## Troubleshooting

To check if the device is recognized by the PC, run `lsusb` to list all USB devices connected to your PC. If the Newport device 
is not found, try connecting the USB cable from the device to your PC first and then restart the device.