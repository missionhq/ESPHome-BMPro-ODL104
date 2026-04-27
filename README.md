# ESPHome-BMPro-ODL104

This ESPHome configuration is used to connect a BMPro RV Battery Management system to Home Assistant in order to mointor a number of sensors including Battery Voltage, Currrent and Battery Level.

It uses bluetooth to connect to a BMPro ODL104 (and possibly a ODL105) CAN-Bus to BLE gateway and bring the Battery Management Systems data into Home Assistant.

This ODL104 uses a single BLE Characteristic Notification to notify all of its data values, with the data containing the packet Type, Data Source, Register ID and payload value as a byte array.

The [bmpro-odl104.yaml](bmpro-odl104.yaml) file receives these notifications, decodes the packet then updates various HA sensors accoridngly

Let me know if you discover any other register values that could be added to the list!

You can view the Protocol format [HERE](protocol.md)

![Dashboard](img/HASS_Dash.jpg "Dashboard Sample")

