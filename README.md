
# esp3pe

esp3pe is a proof of concept for a system consisting Image Recognition & Speech Recognition using the [ESP32-S3-EYE](https://github.com/espressif/esp-who/blob/master/docs/en/get-started/ESP32-S3-EYE_Getting_Started_Guide.md) (ESP32-S3 SoC) development kit




## Installation

We'll need the [Espressif IoT Development Framework](https://github.com/espressif/esp-idf) (SDK) to start developing:
1. Clone SDK :

```bash
git clone https://github.com/espressif/esp-idf.git
```

2. Because we're using the ESP32-S3 SoC, checkout to a tag of a version that supports the SoC. Refer to [ESP-IDF Release and SoC Compatibility](https://github.com/espressif/esp-idf#esp-idf-release-and-soc-compatibility) for details. As of the time of writing this doc, `v5.1-dev` is being used

3. Install the toolchain needed for cross compilation. The SDK already has a bash script for downloading and installing the necessary toolchain to `~/.espressif`:
```bash
cd /path/to/esp-idf
./install.sh
```
4. Export the paths to our toolchain. This has to done to every terminal that we're using for compiling, flashing, modifying configs, etc.:
```bash
cd /path/to/esp-idf
source export.sh
```
or to make it a bit easier, add the following to `~/.bashrc`:
```bash
alias get-idf='source /home/lamht/work/dev/esp-idf/export.sh'
```
then `source ~/.bashrc` or create a new terminal for this to take effect. After that, running `get-idf` would be sufficient for exporting the necessary paths. `esp-idf` uses a wrapper tool written in Python for every operation (building, flashing ,etc.) called `idf.py`


## Building & Flashing

To build the project:

1. Clone the project
```bash
git clone https://github.com/NunkLabs/esp3pe.git
```

2. Configure the target (in this case we're using ESP32-S3):
```bash
cd /path/to/esp3pe
idf.py --preview set-target esp32s3
```

3. Build the project:
```bash
idf.py build
```

4. (Optional) For adding more components to our project or configure:
```bash
idf.py menuconfig
```
and make the necessary changes. This should change the file `sdkconfig`. **Always do this making SDK-related changes. Do not modify `sdkconfig` directly**

5. Flash the binary to our board via serial port:
```bash
idf.py -p <serial port> flash
```




## Monitoring Logs

We can simply use the wrapper tool for monitoring logs:
```bash
idf.py monitor
```

or we can use any other serial communication program (e.g. [minicom](https://wiki.emacinc.com/wiki/Getting_Started_With_Minicom), [picocom](https://picocom.com/), etc.) and point it to use the serial port of our board

## Tips & Gotchas

#### Determining the serial port of our board

Use `dmesg` to figure out which port is being used after plugging the board onto your machine. For example:
```bash
[ 7304.177968] usb 1-13: new full-speed USB device number 16 using xhci_hcd
[ 7304.327190] usb 1-13: New USB device found, idVendor=303a, idProduct=1001, bcdDevice= 1.01
[ 7304.327203] usb 1-13: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 7304.327210] usb 1-13: Product: USB JTAG/serial debug unit
[ 7304.327215] usb 1-13: Manufacturer: Espressif
[ 7304.327219] usb 1-13: SerialNumber: 68:B6:B3:22:26:C8
[ 7304.331274] cdc_acm 1-13:1.0: ttyACM0: USB ACM device
```
so `/dev/ttyACM0` is being used in this machine

#### Fatal error occurred: Could not open <serial port>, the port doesn't exist

`sudo chown ${USER} <serial port>` should resolve this
## Documentation

* [ESP-IDF Programming Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/get-started/index.html)
* [Espressif IoT Development Framework](https://github.com/espressif/esp-idf)
* [ESP-WHO](https://github.com/espressif/esp-who)
