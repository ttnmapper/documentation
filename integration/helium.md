# Helium integration

A HTTP Integration allows you to upload data to TTN Mapper. 

The goal of TTN Mapper is to provide a map of the actual coverage of LoraWAN Hotspots. Contributors to TTN Mapper measure the performance of Hotspots in their vicinity and upload this information to the TTN Mapper website. Here the information is aggregated and shared with the TTN community.

Go to https://ttnmapper.org for the global coverage map.


## Prerequisites

In order to use TTN Mapper a LoRaWAN device with a GPS, capable of transmitting its GPS coordinates, is required. The minimal location information that needs to be sent by the device is its latitude and longitude. Preferably it should also send its altitude and HDOP values. If HDOP is not available, the end device should provide the accuracy of the GPS fix (in meters). As a last resort, if no accuracy can be provided, the satellite count can be sent. Devices that do not report a location are still used to determine if a Hotspot is online or not.

Once the integration is enabled any message sent by the end device will also be published to the TTN Mapper website. In order for TTN Mapper to correctly interpret the incoming messages, a payload decoder needs to be configured. 
* If your device is sending data in Cayenne LPP format, choose this decoder. 
* Otherwise create a custom javascript decoder which decoded the raw payload into one of these formats:
  1. A JSON object containing the keys "**latitude**", "**longitude**" and "**altitude**". Some variations of these keys will also work. 
  2. In addition the JSON object should preferably contain one of the following keys "**hdop**", "**accuracy**" or "**sats**".

> As an alternative the payload decoder for [Helium Cargo](https://docs.helium.com/use-the-network/console/integrations/cargo/) or [Helium Mappers](https://docs.helium.com/use-the-network/coverage-mapping/mappers-quickstart/)mappers can be used as well with TTN Mapper. 

If you are developing your own GPS enabled LoRa device please check the following [Github repository](https://github.com/ttnmapper/gps-node-examples) for example end-device software and decoder functions to be used with them.

## Setting up Helium for TTN Mapper

Following steps shall be followed to setup Helium for TTN Mapper:

Assuming you created a device and setup a payload decoder the meets the requirements as stated before:
1. Create integration
2. Create label and link devices
3. Configure your TTN Mapper application in **Flows**

### 1. Create the integration

On Helium console, click on the *add (+)* icon in the bottom left and select **Add integrations**. Then click on HTTP. In the configuration page for the integration fill in the following:

* **Endpoint URL (Required)**: Enter `https://integrations.ttnmapper.org/helium/v1`
* **Step 3 - Name your Integration (Required)**: Enter the name for your integration
* **HTTP Headers (Required)**: Add header **TTNMAPPERORG-USER** and set your email address in this header to allow TTN Mapper to associate your coverage data with you. This will allow to easily find your data, and also verify when you request your data to be deleted.
* Click **Add Integration** to confirm and save your integration.

#### Optional Headers

* **TTNMAPPERORG-EXPERIMENT**: You only need to provide an experiment name if you are testing and do not want your mapping results to contribute to the main coverage map for the network. In other words, when you are mapping normally with a device that is between 1 and 2 metre above ground level, you can leave this field blank. If you are doing anything else like launching and tracking a balloon, please provide a unique experiment name here. Adding the date to the experiment name helps to find it again later. Also see [Experiments](#experiments).

## 2. Create label and link devices

On Helium console:
* click on the **add (+)** icon in the bottom left and select **Add label**. Enter the label name, eg. _ttnmapper_.
* click on devices, select the device that will be used for TTN Mapper and add label **ttnmapper** to the device.

## 3. Configure your TTN Mapper application in 'Flows' 

On Helium console, click **Flows** in the upper left corner.

* Add nodes to the workspace:
  * drag device group with _label_ **ttnmapper** to the workspace
  * drag the _function_ with the payload decoder you will use for TTN Mapper to the workspace
  * drag _integration_ **ttnmapper** to the workspace
* Link label **ttnmapper** (blue) to your Function (purple)
* Link label Function (purple) to the integration **ttnmapper** (blue)
* Open inspector for _label_ **ttnmapper** and verfy if the device is linked to the _label_.

### Configuring Multiple Packets

To be able to see as much Hotspots as possible all available packets shall be purchased. Therefore configure [Multiple Packets](https://docs.helium.com/use-the-network/console/adr/#multiple-packets) for the device group.

## Verify the integration is working correctly

In order to verify whether the integration has been configured correctly, go to the Live Data page for your device on the Console. Switch on your device and make sure you see data appearing in console. Now go to the TTN Mapper website and in the menu select "[Advanced maps](https://ttnmapper.org/advanced-maps/)". In the "Device data" section fill in the Device ID field. In the Start Date and End Date fields choose today. Click on "View map" and you should see the data points sent by your end device.

> TTN Mapper's support for Helium is still a work in progress and therefore not all maps will show the collected mapping data. Using the per-device raw data map and csv is the most reliable way to make sure your data is accepted into the TTN Mapper database.

## Experiments

### When should I use an "experiment" to map my coverage?

An experiment is a way to keep unrealistic coverage measurements away from the main map. Experiments should be used when testing new hardware or coverage is mapped from aeroplanes, balloons or any similar unrealistic altitudes.

In other words logging to the main map should only be done from roughly 0.5m-2m above ground level. "Ground level" should be interpreted as any place easily accessible by a human - or any place where an IoT device would commonly be installed. The top of a skyscraper is only acceptable if the skyscraper has a viewing deck that is publicly accessible. Man made hills and natural mountains are acceptable. The roof of a car or small delivery truck is fine. The roof of a bus or 14 wheeler truck is not as that is not a average acceptable height at which a sensor will be installed. The dashboard of a truck or bus is however roughly 2m above ground and therefore acceptable.

### Enable or disable logging to an experiment

* To log to an experiment, we need to add a header to the HTTP integration which will tell TTN Mapper to log the data to an experiment.
* To stop logging to an experiment, delete the `TTNMAPPERORG-EXPERIMENT` header entry.
