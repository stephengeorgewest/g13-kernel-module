Backlight kind of works in g15/g510 format, but they don't do the expected things. Aparently not as alike to the G15 as hoped.

<pre><code>stephen@ao751h:~/Documents/projects/g13$ make
make -C /lib/modules/5.14.0-2-686-pae/build M=/home/stephen/Documents/projects/g13 modules
make[1]: Entering directory '/usr/src/linux-headers-5.14.0-2-686-pae'
make[1]: Leaving directory '/usr/src/linux-headers-5.14.0-2-686-pae'
stephen@ao751h:~/Documents/projects/g13$ sudo insmod ./hid-lg-g15.ko
</code></pre>
check dmesg:
<pre><code>...
[141992.481805] usb 2-1: USB disconnect, device number 6
[141993.497565] usb 2-1: new full-speed USB device number 7 using uhci_hcd
[141993.688838] usb 2-1: New USB device found, idVendor=046d, idProduct=c21c, bcdDevice= 2.03
[141993.688879] usb 2-1: New USB device strings: Mfr=0, Product=1, SerialNumber=0
[141993.688896] usb 2-1: Product: G13
[141993.713546] hid-generic 0003:046D:C21C.0130: hiddev2,hidraw4: USB HID v1.11 Device [G13] on usb-0000:00:1d.0-1/input0
[142482.986330] lg-g15 0003:046D:C21C.0130: hidraw4: USB HID v1.11 Device [G13] on usb-0000:00:1d.0-1/input0
[142483.055391] input: Logitech Gaming Keyboard Gaming Keys as /devices/pci0000:00/0000:00:1d.0/usb2/2-1/2-1:1.0/0003:046D:C21C.0130/input/input47
</code></pre>
Check leds by writing to:
<pre><code>root@ao751h:/home/stephen/Documents/projects/g13# echo 0 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# echo 128 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# echo "#0000ff" > /sys/class/leds/g15\:\:kbd_backlight/color
root@ao751h:/home/stephen/Documents/projects/g13# echo 255 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# echo 64 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# echo 0 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# echo 255 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# echo 64 > /sys/class/leds/g15\:\:lcd_backlight/brightness
root@ao751h:/home/stephen/Documents/projects/g13# cat /sys/class/leds/g15\:\:macro_record/brightness 
1
root@ao751h:/home/stephen/Documents/projects/g13# echo 0 >  /sys/class/leds/g15\:\:macro_record/brightness
</code></pre>

Check button presses with evtest (doesn't work yet?)

<pre><code>root@ao751h:/home/stephen/Documents/projects/g13# ls /sys/devices/pci0000\:00/0000\:00\:1d.0/usb2/2-1/2-1\:1.0/0003\:046D\:C21C.0130/input/input47/
capabilities/ device/       event12/      id/           inhibited     modalias      name          phys          power/        properties    subsystem/    uevent        uniq
root@ao751h:/home/stephen/Documents/projects/g13# ls /sys/devices/pci0000\:00/0000\:00\:1d.0/usb2/2-1/2-1\:1.0/0003\:046D\:C21C.0130/input/input47/^C
root@ao751h:/home/stephen/Documents/projects/g13# evtest
No device specified, trying to scan all of /dev/input/event*
Available devices:
/dev/input/event0:      AT Translated Set 2 keyboard
/dev/input/event1:      Lid Switch
/dev/input/event2:      Power Button
/dev/input/event3:      Sleep Button
/dev/input/event4:      Video Bus
/dev/input/event5:      SynPS/2 Synaptics TouchPad
/dev/input/event6:      Logitech G602
/dev/input/event7:      PC Speaker
/dev/input/event8:      WebCam: Webcam
/dev/input/event9:      HDA Digital PCBeep
/dev/input/event10:     HDA Intel MID Mic
/dev/input/event11:     HDA Intel MID Headphone
/dev/input/event12:     Logitech Gaming Keyboard Gaming Keys
Select the device event number [0-12]: 12
Input driver version is 1.0.1
Input device ID: bus 0x3 vendor 0x46d product 0xc21c version 0x111
Input device name: "Logitech Gaming Keyboard Gaming Keys"
Supported events:
  Event type 0 (EV_SYN)
  Event type 1 (EV_KEY)
    Event code 656 (?)
    Event code 657 (?)
    Event code 658 (?)
    Event code 659 (?)
    Event code 660 (?)
    Event code 661 (?)
    Event code 662 (?)
    Event code 663 (?)
    Event code 664 (?)
    Event code 665 (?)
    Event code 666 (?)
    Event code 667 (?)
    Event code 668 (?)
    Event code 669 (?)
    Event code 670 (?)
    Event code 671 (?)
    Event code 672 (?)
    Event code 673 (?)
    Event code 688 (?)
    Event code 691 (?)
    Event code 692 (?)
    Event code 693 (?)
    Event code 696 (?)
    Event code 697 (?)
    Event code 698 (?)
    Event code 699 (?)
    Event code 700 (?)
Properties:
Testing ... (interrupt to exit)
Event: time 1634308924.628871, type 1 (EV_KEY), code 656 (?), value 1
Event: time 1634308924.628871, -------------- SYN_REPORT ------------
Event: time 1634308924.738877, type 1 (EV_KEY), code 656 (?), value 0
Event: time 1634308924.738877, -------------- SYN_REPORT ------------
Event: time 1634308925.894912, type 1 (EV_KEY), code 656 (?), value 1
Event: time 1634308925.894912, -------------- SYN_REPORT ------------
Event: time 1634308926.030915, type 1 (EV_KEY), code 656 (?), value 0
Event: time 1634308926.030915, -------------- SYN_REPORT ------------
Event: time 1634308926.540922, type 1 (EV_KEY), code 656 (?), value 1
Event: time 1634308926.540922, -------------- SYN_REPORT ------------
Event: time 1634308927.098919, type 1 (EV_KEY), code 657 (?), value 1
Event: time 1634308927.098919, -------------- SYN_REPORT ------------
Event: time 1634308927.674956, type 1 (EV_KEY), code 656 (?), value 0
Event: time 1634308927.674956, -------------- SYN_REPORT ------------
Event: time 1634308928.206964, type 1 (EV_KEY), code 657 (?), value 0
Event: time 1634308928.206964, -------------- SYN_REPORT ------------
Event: time 1634308929.591008, type 1 (EV_KEY), code 691 (?), value 1
Event: time 1634308929.591008, -------------- SYN_REPORT ------------
Event: time 1634308929.729016, type 1 (EV_KEY), code 691 (?), value 0
Event: time 1634308929.729016, -------------- SYN_REPORT ------------
Event: time 1634308929.915016, type 1 (EV_KEY), code 692 (?), value 1
Event: time 1634308929.915016, -------------- SYN_REPORT ------------
Event: time 1634308930.059011, type 1 (EV_KEY), code 692 (?), value 0
Event: time 1634308930.059011, -------------- SYN_REPORT ------------
Event: time 1634308930.279027, type 1 (EV_KEY), code 693 (?), value 1
Event: time 1634308930.279027, -------------- SYN_REPORT ------------
Event: time 1634308930.444437, type 1 (EV_KEY), code 693 (?), value 0
Event: time 1634308930.444437, -------------- SYN_REPORT ------------
Event: time 1634308931.379056, type 1 (EV_KEY), code 693 (?), value 1
Event: time 1634308931.379056, -------------- SYN_REPORT ------------
Event: time 1634308931.571060, type 1 (EV_KEY), code 693 (?), value 0
Event: time 1634308931.571060, -------------- SYN_REPORT ------------
Event: time 1634308931.821073, type 1 (EV_KEY), code 688 (?), value 1
Event: time 1634308931.821073, -------------- SYN_REPORT ------------
Event: time 1634308932.009075, type 1 (EV_KEY), code 688 (?), value 0
Event: time 1634308932.009075, -------------- SYN_REPORT ------------
Event: time 1634308933.693122, type 1 (EV_KEY), code 696 (?), value 1
Event: time 1634308933.693122, -------------- SYN_REPORT ------------
Event: time 1634308933.889132, type 1 (EV_KEY), code 696 (?), value 0
Event: time 1634308933.889132, -------------- SYN_REPORT ------------
^Croot@ao751h:/home/stephen/Documents/projects/g13#
</code></pre>
