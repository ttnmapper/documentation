# Project Status

TTN Mapper is currently undergoing a major refactor to support The Things Stack (v3). To fully support TTS, TTN Mapper needs to be able to support multiple networks. 
This is a big change to the backend systems, and has already started in December 2018. The current major focus is uniquely identifying a network and gateways. After this is sorted out the focus will shift to the website.

## General

- [ ] Simulation based coverage - research topic
- [ ] Cross platform mobile app (Flutter) - help wanted
- [ ] Website rewrite in JS framework - help wanted
- [ ] Website user logins, network claims, private network subscriptions - help wanted

## The Things Network V2

- [x] Data accepted and stored in TTN Mapper's database
- [x] Android app support
- [x] iOS app support
- [x] Integration: available on v2 console
- [x] Device raw data: available
- [x] Gateway raw data: available
- [x] Gateway markers: available
- [x] Gateway online/offline status: available
- [x] Radar plots: available
- [x] Heatmap: available as a combined heatmap for all networks
- [x] Area plots: available

## The Things Stack

- [x] Data accepted and stored in TTN Mapper's database
- [x] Android app support
- [ ] iOS app support: not yet implemented - help needed
- [x] Webhook Template: available on v3 console. See [V3 integration](integration/tts-integration-v3.md).
- [x] Device raw data: Using the Advanced maps, one can view your device's raw data on the map. This will only show non-experimental points. The CSV option for per-device raw data will show experimental and non-experimental data.
- [ ] Experiment raw data: not yet implemented
- [ ] Gateway raw data: Not yet implemented
- [ ] Gateway markers: Currently pulling from V2. If gateway ID on V3 matches the ID on V2, the V2 gateway location will be used to draw the marker.
- [ ] Gateway status: Will use status of V2 network if gateway ID matches.
- [ ] Radar plots: not yet implemented
- [x] Heatmap: Data from all networks are currently used to draw heatmaps. This needs to change to be a per-network heatmap.
- [ ] Area plots: not yet implemented

### Issues
* Network identification

## Chirp Stack

- [x] Data accepted and stored in TTN Mapper's database
- [x] Android app support - partial, as network identification needs to be refactored
- [ ] iOS app support
- [x] Integration: TTN Mapper will ingest data from ChirpStack if a webhook integration has been set up, according to [ChirpStack integration](integration/chirpstack.md).
- [x] Device raw data: available
- [ ] Gateway raw data: not yet implemented
- [x] Gateway markers: partially available
- [ ] Gateway status: unsure if this is available
- [ ] Radar plots: not yet implemented
- [ ] Heatmap: Data from all networks are currently used to draw the heatmap.
- [ ] Area plots: not yet implemented

### Issues

* Identifying a network: https://github.com/brocaar/chirpstack-network-server/issues/532

## AWS IoT Core for LoRaWAN

Not yet supported.

### Issues

No easy way to get data out to a REST API and on an MQTT broker.

## Amazon Sidewalk

Not yet supported

## Helium

Not supported, and likely no need due to https://mappers.helium.com/

## Sigfox

Not supported, and will likely not be supported due to licensing restrictions.
