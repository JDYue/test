# Android Things Cloud IoT MQTT Example

This Android Things activity demonstrates a connection between an
Android Things device and Google's Cloud IoT service.

## Pre-requisites

- Android Things compatible board
- Android Studio 2.2+
- Properly configured Cloud IoT setup

## Build, Configure, and Activate

### Build and Install

On Android Studio, select the module in the select box by the "Run" button, and then click on the
"Run" button. This will run the test activity as well as install, but it won't do anything yet
because it hasn't been configured.

If you prefer to build on the command line, type

```bash
./gradlew installDebug
```

### Monitor and Logging

To monitor progress of the example activity, use the adb logcat command:

```bash
adb logcat CloidIot:D *:S
```

### Generate Device Key

To generate a private device key, use the following steps (for ES-256 key). See
the
[CloudIoT Device Security Page](https://cloud.google.com/iot/docs/device_security)
for full documentation on this process. After generating the appropriate
`pkcs8` file, copy it to the target location for test and/or configure access.

```bash
PRIVATE_KEY_DIR=app/src/androidTest/assets/CloudIot/
mkdir -p $PRIVATE_KEY_DIR
cp ec_private_pkcs8 $PRIVATE_KEY_DIR
```

### Configure Connection Parameters

The basic commands to configure the device consist of pushing the generated key file to
device storage, and then running the configuration intent to set various parameters. Once done,
the configuration is saved and so this step does not need to be repeated each time. Note that
this assumes that WiFi or other network connection has already been configured on the device,
per the basic Android Things setup procedure.

```bash
DEVICE_STORAGE_DIR=/storage/emulated/0/CloudIot/
adb shell mkdir -p "${DEVICE_STORAGE_DIR}"
adb push ec_private_pkcs8 "${DEVICE_STORAGE_DIR}"

# PROJECT_ID is the Cloud Project ID to use.
# REGISTRY_ID is the Cloud IoT Device Registry name.
# DEVICE_ID is the specific device id within the registry.
adb shell am start -n com.example.androidthings.mqtt/.CloudIotMqttActivity \
  -a com.example.androidthings.mqtt.CONFIGURE \
  -e project_id "${PROJECT_ID}" -e registry_id "${REGISTRY_ID}" -e device_id "${DEVICE_ID}" \
  -e algorithm ES256 -e private_key_file CloudIot/ec_private_pkcs8
```

If you are using Linux, there is a handy script `configure.sh` that does the above steps for you.
You just need to execute `configure.sh` and follow the instructions.

```bash
./configure.sh
```

### Run

On Android Studio, select the module in the select box by the "Run" button, and then click on the
"Run" button.

If you prefer to run on the command line, type

```bash
adb shell am start com.example.androidthings.mqtt/.CloudIotMqttActivity
```

## License

Copyright 2017 The Android Open Source Project, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
