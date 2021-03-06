# brother_printer

WORK IN PROGRESS

Brother printer SDK for Flutter using native SDK v4 on iOS and v3 on Android.

## Getting Started

This project is a starting point for a Flutter
[plug-in package](https://flutter.dev/developing-packages/),
a specialized package that includes platform-specific implementation code for
Android and/or iOS.

For help getting started with Flutter, view our 
[online documentation](https://flutter.dev/docs), which offers tutorials, 
samples, guidance on mobile development, and a full API reference.

## Installation

Be sure to read requirements of the native SDK:
https://support.brother.com/g/s/es/htmldoc/mobilesdk/

### iOS

In the `Podfile` uncomment:

```
    platform :ios, '9.0'
```

In the `Info.plist` add:

```
    <key>NSBluetoothAlwaysUsageDescription</key>
    <string>Bluetooth is required for connection to printers.</string>
    <key>NSBluetoothPeripheralUsageDescription</key>
    <string>Bluetooth is required for connection to printers.</string>
    <key>NSLocalNetworkUsageDescription</key>
    <string>${PRODUCT_NAME} uses the local network to discover and printers on your network.</string>
    <key>UISupportedExternalAccessoryProtocols</key>
    <array>
        <string>com.brother.ptcbp</string>
    </array>
    <key>NSBonjourServices</key>
    <array>
        <string>_printer._tcp</string>
    </array>
```

#### Warning

Since iOS 14 the detection of printers with BrotherSDK via local network doesn't work (event with "local network" permission).
In replacement, mDNS protocol is used to detect printers.

### Android

Connection doesn't seem to work on emulator.

Add permissions:

```
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```

You have to manually authorized the location permission on the device (will be improve later).

#### Warning

You may have some crash in release mode, you can add this to your `android/app/build.gradle`:

```
    buildTypes {
        release {
            minifyEnabled false
            shrinkResources false

            signingConfig signingConfigs.release
        }
    }
```


## Notes

There are difference between what return the iOS version and the Android version

```
    print('${device.model.nameAndroid} - ${device.source} - ${device.modelName} - ${device.ipAddress} - ${device.printerName} - ${device.macAddress} - ${device.nodeName} - ${device.serialNumber} - ${device.bleAdvertiseLocalName}');

    # iOS
    QL-820NWB - BrotherDeviceSource.network - Brother QL-820NWB - 10.0.0.1 - Brother QL-820NWB - b0:68:e6:97:db:42 - BRWB068E697DB42 - K9Z195606 - null
    QL-820NWB - BrotherDeviceSource.bluetooth - QL-820NWB - null - QL-820NWB5606 - null - null - 806FB0BABE7C - null

    Network: modelName, ipAddress, printerName, network macAddress, nodeName, serialNumber
    Bluetooth: modelName, printerName, serialNumber

    # Android
    QL-820NWB - BrotherDeviceSource.network - Brother QL-820NWB - 10.0.0.1 - null - B0:68:E6:97:DB:42 - BRWB068E697DB42 - null - null
    QL-820NWB - BrotherDeviceSource.bluetooth - null - null - QL-820NWB5606 - 80:6F:B0:BA:BE:7D - null - null - null

    Network: modelName, ipAddress, network macAddress, serialNumber
    Bluetooth: printerName, bluetooth macAddress
```

## Author

- [Jonathan VUKOVICH-TRIBOUHARET](https://github.com/jonathantribouharet)

