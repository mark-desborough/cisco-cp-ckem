# The Cookie Crumbs

Cisco signature stripped from the `cmterm-88xx-sip.14-0-1-0001-135.k3.cop.sgn` the multi phone firmware (not encrypted with sebn filesystems):
With something like the first 420 bytes stripped off with dd. 

then renamed and extracted like a tar.gz file: 
`tar -xzvf cmterm-88xx-sip.14-0-1-0001-135.k3.cop.tar.gz`

rootfs from the binary 
`mount rootfs88xx.14-0-1-0001-135.sbn /mnt/rootfs -t squashfs -o loop`

files under `etc/` (I assume this is the phone support list): 
- 8861-capabilities
- 8851NR-capabilities
- 8851-capabilities
- 8841-capabilities
- 8811-capabilities

From the kem kernel module (`lib/modules/3.0.31/extra/lkem.ko` and `lib/modules/3.0.31/extra/lkem.ko`):
- `alias usb:v1836p1235d*dc*dsc*dp*ic*isc*ip* lkem`
- `alias usb:v0698pE8E9d*dc*dsc*dp*ic*isc*ip* lkem`

# Debug Trace from cisco forum: 

Cisco 8851 phone running SIP firmware sip88xx.14-3-1-0201-246, with a Key Expansion Module (CP-8800-A-KEM) attached.

```
DEBUG>  dmesg -w
...
[34275.093994] lkem: lkem_intr_in_callback: nonzero intr-in status received: -110
[34275.093994] lkem: lkem_intr_in_callback: usb_submit_urb(intr-in) failed (-19)
[34275.095672] usb 4-1: USB disconnect, device number 123
[34275.095703] usb 4-1.2: USB disconnect, device number 124
[34275.096038] /home/slbuild/master/ip_sl/infra/drivers/bigeasy/lkem/lkem_driver.c: LKEM0 now disconnected
[34277.755523] Indeed it is in host mode hprt0 = 00021501
[34277.935455] usb 4-1: new full speed USB device number 125 using dwc_otg
[34277.936370] INFO:: dwc_otg_hcd_handle_hc_n_intr: (chhltd,0) no qtd for channel 11, urb=  (null)
[34277.936645] Indeed it is in host mode hprt0 = 00021501
[34278.137176] usb 4-1: not running at top speed; connect to a high speed hub
[34278.141296] usb 4-1: New USB device found, idVendor=05e3, idProduct=0608
[34278.141326] usb 4-1: New USB device strings: Mfr=0, Product=1, 
[34278.141357] usb 4-1: Product: USB2.0 Hub
[34278.141876] usb 4-1: cisco - usb product=USB2.0 Hub
[34278.141906] usb 4-1: cisco - usb dev_name=4-1 config_num=1
[34278.141967] usb 4-1: cisco - product=USB2.0 Hub
[34278.141998] usb 4-1: cisco - other device (class=9)
[34278.142028] usb 4-1: cisco - hub
[34278.142059] usb 4-1: cisco - enable
[34278.143493] hub 4-1:1.0: USB hub found
[34278.144287] hub 4-1:1.0: 4 ports detected
[34289.895294] usb 4-1.2: new full speed USB device number 126 using dwc_otg
[34290.001159] usb 4-1.2: New USB device found, idVendor=1836, idProduct=1235
[34290.001159] usb 4-1.2: New USB device strings: Mfr=1, Product=2, 
[34290.001190] usb 4-1.2: Product: KEM
[34290.001190] usb 4-1.2: Manufacturer: FOX
[34290.001190] usb 4-1.2: SerialNumber: 3
[34290.001525] usb 4-1.2: cisco - usb product=KEM
[34290.001525] usb 4-1.2: cisco - usb dev_name=4-1.2 config_num=1
[34290.001556] usb 4-1.2: cisco - product=KEM
[34290.001556] usb 4-1.2: cisco - other device (class=255)
[34290.001556] usb 4-1.2: cisco - disable
[34290.015289] /home/slbuild/master/ip_sl/infra/drivers/bigeasy/lkem/lkem_driver.c: USB lkem device now attached to USBlkem-0
[34303.321777] lkem: lkem_intr_in_callback: nonzero intr-in status received: -110
[34303.321777] lkem: lkem_intr_in_callback: usb_submit_urb(intr-in) failed (-19)
[34303.343475] usb 4-1: USB disconnect, device umber 125
[34303.343505] usb 4-1.2: USB disconnect, device number 126
[34303.343811] /home/slbuild/master/ip_sl/infra/drivers/bigeasy/lkem/lkem_driver.c: LKEM0 now disconnected
DEBUG> 
```

```
6869 DEB Apr 29 21:12:40.411337 (468-686) DMAN-handleNewDevice(): Got VendorID= 0x1836 ProductID= 0x1235
6870 NOT Apr 29 21:12:40.411362 (468-686) DMAN-isdeviceAOM(): LKEM Detected
```

# The Physical Facts

My `cp-ckem-c=` reports as 2 devices (the Cypress hub and the asic): 

```
Bus 001 Device 055: ID 04b4:6560 Cypress Semiconductor Corp. CY7C65640 USB-2.0 "TetraHub"
Bus 001 Device 056: ID 0698:e8e9 Chuntex (CTX) Longsword KEM
```

```
Bus 001 Device 056: ID 0698:e8e9 Chuntex (CTX) Longsword KEM
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               1.10
  bDeviceClass          255 Vendor Specific Class
  bDeviceSubClass       255 Vendor Specific Subclass
  bDeviceProtocol       255 Vendor Specific Protocol
  bMaxPacketSize0         8
  idVendor           0x0698 Chuntex (CTX)
  idProduct          0xe8e9 Longsword KEM
  bcdDevice            1.05
  iManufacturer          25 Cisco Systems, Inc.
  iProduct               42 Longsword KEM
  iSerial               101 0123456789.0123456789.0123456789
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x0027
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration        251 Longsword Function
    bmAttributes         0xc0
      Self Powered
    MaxPower                2mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           3
      bInterfaceClass       255 Vendor Specific Class
      bInterfaceSubClass    255 Vendor Specific Subclass
      bInterfaceProtocol    255 Vendor Specific Protocol
      iInterface            251 Longsword Function
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x01  EP 1 OUT
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0040  1x 64 bytes
        bInterval               0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x82  EP 2 IN
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0040  1x 64 bytes
        bInterval               0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0040  1x 64 bytes
        bInterval               5
Device Status:     0x0001
  Self Powered
```

## The Reset Sequence

This reset sequence worked on my `cp-ckem-c=` model (I couldn't find anything to describe the sequence for it). I am unable to find the reset sequence for the dual-lcd versions.

```
Reset the Single LCD Screen Key Expansion Module (8800 only)

If you are having technical difficulties with your Cisco IP Phone 8800 Key Expansion Module, you can reset the module to the factory default settings.
Procedure

Step 1: Restart the key expansion module by disconnecting the power source, waiting a few seconds, and then reconnecting it.

Step 2: As the key expansion module powers up, press and hold Page 1. As the LCD screen turns white, continue pressing Page 1 for at least one second.

Step 3: Release Page 1. The LEDs turn red.

Step 4: Immediately press Page 2 and continue pressing Page 2 for at least one second.

Step 5: Release Page 2. The LEDs turn amber.

Step 6: Press Lines 5, 14, 1, 18, 10, and 9 in sequence.

The LCD screen turns blue. A spinning icon is displayed in the center of the screen.

The key expansion module resets.
```

## wkem vs ckem firmware files: 
binwalk -e on wkem.14-0-1-0001-135.bin comes up with a squashfs. The squashfs is a small linux install for the kernel `3.4.110-rt140-v1.3.5-rc3`
modules compiled for `3.4.110-rt140-v1.3.5-rc3+ preempt mod_unload ARMv5 p2v8`
It has 2 modules. It appears `g_serial.ko` manages a serial USB device. While `g_kem.ko` references: 
```
pxa25x_udc
usb ep addr 0x%x
failed to override string ID
composite
%s: Unexpected call
unconfigured
%s config #%d: %s
%s: Delayed status not supported for w_length != 0
net2280
ep-e
ep-f
goku_udc
ep3-bulk
ep2-bulk
bInterfaceNumber: id %d
%s: can't autoconfigure on %s
Support usb high speed hardware.
&kem->lock
&kem->write_queue
&kem->read_queue
Error: kem cdev_add failed
%s%d
%s %s with %s
userspace failed to provide iSerialNumber
%s ready
iInterface, iConfiguration: id %d
kem device setup error!
dummy_udc
omap_udc
pxa27x_udc
s3c2410_udc
at91_udc
imx_udc
musb-hdrc
atmel_usba_udc
fsl-usb2-udc
amd5536udc
m66592_udc
fsl_qe_udc
ci13xxx_pci
langwell_udc
r8a66597_udc
s3c-hsotg
pch_udc
ci13xxx_msm
renesas_usbhs_udc
s3c-hsudc
net2272
dwc3-gadget
%s, version: FOX KEM 201609
```

