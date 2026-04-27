# ESPHome-BMPro-ODL104

This ESPHome configuration will connect to a BMPro ODL105 (and possibly a ODL105) CAN-Bus to BLE gateway and bring the Battery Management Systems data into Home Assistant.

The ODL104 uses a single BLE Characteristic Notification to notify all of its data values, with the data containing the packet Type, Data Source, Register ID and payload value as a byte array....

View the Protocol format HERE