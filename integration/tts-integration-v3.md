# The Things Stack integration

The TTN Mapper Webhook Template allows you to create a webhook integration for uploading data to TTN Mapper. 

The goal of TTN Mapper is to provide a map of the actual coverage of the TTN gateways. Contributors to TTN Mapper measure the performance of gateways in their vicinity and upload this information to the TTN Mapper website. Here the information is aggregated and shared with the TTN community.

Go to https://ttnmapper.org for the global coverage map.

A video showing how to register and configure a GPS tracker to feed data to TTN Mapper can be found here: https://youtu.be/8KcHVzY-Yng

## Prerequisites

In order to use the TTN Mapper integration a LoRaWAN device with a GPS, capable of transmitting its GPS coordinates, is required. The minimal location information that needs to be sent by the device is its latitude and longitude. Preferably it should also send its altitude and HDOP values. If HDOP is not available, the end device should provide the accuracy of the GPS fix (in meters). As a last resort, if no accuracy can be provided, the satellite count can be sent. Devices that do not report a location are still used to determine of a gateway is online or not.

*Note*: If you don't have a GPS enabled LoRa device, you can still contribute to TTN Mapper using your smartphone (Android or iOS) and any LoRaWAN device. See the "[Using TTN Mapper on Android](https://www.thethingsnetwork.org/labs/story/using-ttnmapper-on-android)" lab. If you want to use the Things Node for mapping check out the "[Mapping gateway coverage using a Things Node](https://www.thethingsnetwork.org/labs/story/mapping-gateway-coverage-using-a-things-node)" lab.

Once the integration is enabled any message sent by the end device will also be published to the TTN Mapper website. In order for TTN Mapper to correctly interpret the incoming messages, a payload decoder needs to be configured on the console. 
* If your device is sending data in Cayenne LPP format, choose this decoder. 
* Otherwise create a custom javascript decoder which decoded the raw payload into one of these formats:
  1. A JSON object containing the keys "**latitude**", "**longitude**" and "**altitude**". Some variations of these keys will also work. 
  2. In addition the JSON object should preferably contain one of the following keys "**hdop**", "**accuracy**" or "**sats**".

If you are developing your own GPS enabled LoRa device please check the following [Github repository](https://github.com/ttnmapper/gps-node-examples) for example end-device software and decoder functions to be used with them.

## Create the integration

On the The Things Stack Console, open your application and then click on the *Integrations* menu on the left. Then click on Webhooks. Search for the TTN Mapper webhook template and click on it. In the configuration page for the integration fill in the following:

* **Webhook ID**: a unique string describing this integration. This is mainly for you to make a distinction between (possibly) multiple integrations you have configured. The value does not have an influence on how the integration works.
* **Email address**: a valid email address. This email address will be used to associate all your data to you, and provides some guarantees on the quality of the data.
* **Experiment name (optional)**: You only need to provide an experiment name if you are testing and do not want your mapping results to contribute to the main coverage map for the network. In other words, when you are mapping normally with a device that is between 1 and 2 metre above ground level, you can leave this field blank. If you are doing anything else like launching and tracking a ballon, please provide a unique experiment name here. Adding the date to the experiment name helps to find it again later. Also see [Experiments](#experiments).

Click on "Create TTN Mapper webhook".

## Fix Base URL

In the list of webhooks, click and open the newly added integration for TTN Mapper. Under Endpoint Settings, there is a Base URL. This value should read `https://integrations.ttnmapper.org/tts/v3`. If it's different, change it and then click save changes at the bottom of the page.

## Verify the integration is working correctly

In order to verify whether the integration has been configured correctly, go to the Live Data page for your device on the Console. Switch on your device and make sure you see data appearing there. Now go to the TTN Mapper website and in the menu select "[Advanced maps](https://ttnmapper.org/advanced-maps/)". In the "Device data" section fill in the Device ID field. In the Start Date and End Date fields choose today. Click on "View map" and you should see the data points sent by your end device.

> TTN Mapper's support for The Things Stack V3 is still a work in progress and therefore not all maps will show the V3 mapping data. Using the per-device raw data map and csv is the most reliable way to make sure your data is accepted into the TTN Mapper database.

For troubleshooting please post your question in the #ttn-mapper channel on Slack.

## Experiments

### When should I use an "experiment" to map my coverage

An experiment is a way to keep unrealistic coverage measurements away from the main map. Experiments should be used when testing new hardware or coverage is mapped from aeroplanes, balloons or any similar unrealistic altitudes.

In other words logging to the main map should only be done from roughly 0.5m-2m above ground level. "Ground level" should be interpreted as any place easily accessible by a human - or any place where an IoT device would commonly be installed. The top of a skyscraper is only acceptable if the skyscraper has a viewing deck that is publicly accessible. Man made hills and natural mountains are acceptable. The roof of a car or small delivery truck is fine. The roof of a bus or 14 wheeler truck is not as that is not a average acceptable height at which a sensor will be installed. The dashboard of a truck or bus is however roughly 2m above ground and therefore acceptable.


### Enable or disable logging to an experiment

To log to an experiment, we need to add a header to the webhook integration which will tell TTN Mapper to log the data to an experiment.

When you create a new webhook from the template you can specify an experiment name, which will automatically add the `TTNMAPPERORG-EXPERIMENT` header to the webhook. If you didn't add an experiment name when you created the webhook but wish to start logging to an experiment, you can delete the webhook and create a new one and specify an experiment name.

Alternatively you can manually add the header by going to your application, Integrations, Webhooks, and open the previously added TTN Mapper Integration. Under Addition Headers, click Add Header Entry. In the first block (where Authorization is displayed in grey) type in `TTNMAPPERORG-EXPERIMENT`. In the second block (where Bearer my-auth-token is displayed in grey) fill in a unique name to identify your experiment.

To stop logging to an experiment, delete the `TTNMAPPERORG-EXPERIMENT` header entry.
