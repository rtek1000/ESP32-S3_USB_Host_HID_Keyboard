# ESP32-S3 USB Host HID Keyboard
Example of ESP32-S3 and support for mini keyboard with built-in touchpad

- Framework ESP-IDF
- Board: ESP32-S3 with USB OTG
- Tested, worked with ESP-IDF v5.3.0

This type of keyboard has 2 HID interfaces, so some microcontrollers cannot recognize it correctly, so when touching the touchpad, errors occur.

[Modification](https://github.com/espressif/esp-idf/issues/12667) of the code 'HID Host' present in the ESP-IDF [examples](https://github.com/espressif/esp-idf/blob/ab03c2ea13ecaac1510b75e93b32cf0c472640fb/examples/peripherals/usb/host/hid/main/hid_host_example.c):

> Regarding the example, right now the HID Driver parses only reports when HID device support BOOT protocol.
> Otherwise, the general output report should be present.
> 
> Based on the USB descriptor and log output you have provided, both interfaces were connected and should work as a HID devices by boot protocol.
> 
> Could you annotate the forcing BOOT protocol for both (Keyboards and Mice) protocols and try the HID example again?
> This could be achieved with the annotating lines 438-443 in hid_host_example.c.

```C++
ESP_ERROR_CHECK(hid_host_device_open(hid_device_handle, &dev_config));
// if (HID_SUBCLASS_BOOT_INTERFACE == dev_params.sub_class) {
//    ESP_ERROR_CHECK(hid_class_request_set_protocol(hid_device_handle, HID_REPORT_PROTOCOL_BOOT));
//    if (HID_PROTOCOL_KEYBOARD == dev_params.proto) {
//        ESP_ERROR_CHECK(hid_class_request_set_idle(hid_device_handle, 0, 0));
//    }
}
ESP_ERROR_CHECK(hid_host_device_start(hid_device_handle));
```

