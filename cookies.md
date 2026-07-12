# The Cookie Crumbs

Cisco signature stripped from the `cmterm-88xx-sip.14-0-1-0001-135.k3.cop.sgn`
something like the first 420 bytes stripped off with dd. 

then renamed and extracted like a tar.gz file: 
`tar -xzvf cmterm-88xx-sip.14-0-1-0001-135.k3.cop.tar.gz`

rootfs from the binary 
`mount rootfs88xx.14-0-1-0001-135.sbn /mnt/rootfs -t squashfs -o loop`

files under /etc/ (I assume this is the phone support list): 
- 8861-capabilities
- 8851NR-capabilities
- 8851-capabilities
- 8841-capabilities
- 8811-capabilities

From the kem kernel module (`lib/modules/3.0.31/extra/lkem.ko` and `lib/modules/3.0.31/extra/lkem.ko`):
- alias usb:v1836p1235d*dc*dsc*dp*ic*isc*ip* lkem
- alias usb:v0698pE8E9d*dc*dsc*dp*ic*isc*ip* lkem

# Debug Trace from cisco forum: 

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
