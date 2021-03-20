# Documentation

This repository tries to document how TTN Mapper works and how the different repositories on this Github organisation fits together.

# Bugtracker

* https://github.com/ttnmapper/bugtracker - All bugs can be reported here or on the respective repository if it is known where the issue is.

# V1 - "old" PHP based system

* https://github.com/ttnmapper/ttnmapper-web - This repo contains everything needed to spin up the PHP-based website on a LAMP stack. It also includes the processing scripts for aggregation of data, which is mostly written in Python. The current public production TTN Mapper runs this stack, along with some extras to make it more scalable.

# V1 to V2 migration path

* https://github.com/ttnmapper/mysql-insert-raw - This service runs as a docker container. It subscribes for new data on a V2 RabbitMQ broker and inserts the data into the MySQL database used by the V1 stack.

# V2 backend microservices

* https://github.com/ttnmapper/ingress-api - This is the api endpoint where data is received from the LoRaWAN server or the mobile phone apps. It's a centralised location through which any incoming data must flow. Data is received from the origin, it is validated and rewritten in a generic TTN Mapper format before it is published on a "new_packets" RabbitMQ fanout exchange.

* https://github.com/ttnmapper/websockets-live - Allows the website to subscribe to a "live" data feed, allowing the lvie tracking of devices like high altitude balloons. This service has been created fro the balloon experiments, but could be used for any live raw data feed.

* https://github.com/ttnmapper/postgres-insert-raw - Inserts all new packets into a postgres database. After the data is added it is re-published on a RabbitMQ fanout exchange "inserted_data" from where the aggregation services can receive it.

* https://github.com/ttnmapper/gateway-update - Listens for all new packets and updates the last seen of gateways in the postgres database. Periodically the gateway list api endpoints of TTN is queried to get the location and description of gateways, as well as the last seen of all other gateways. When a gateway's location changes, and this change is more than 100m, a "gateway move" is registered. A gateway moved triggers old data for the respective gateway to be hidden. To acomplish this the gateway move data is published to a RabbitMQ fanout exchange "gateway_moved".

* https://github.com/ttnmapper/postgres-insert-gridcell - Inserted data is aggregated into signal strength bins for a zoom level 19 slippy map tile, on a per gateway basis. This service subscribes to the "inserted_data" and the "gateway_moved" exchanges. When a gateway moves all previous aggregated data is deleted and all data since the move is used to re-aggregate. The result of this service is so-called gridcells which are used in visualisations like the heatmap.

* https://github.com/ttnmapper/gateway-channels - This service subscribes to the inserted_data exchange. For every packet that is received a counter is incremented on a per gateway-frequency basis. The result is a table of gateway, frequency and number of times heard. This allows one to easily say how many channels a gateway listens on, and also will allow another service to guess which frequnecy plan(s) is used by the gateway.

# V2 Frontend

* https://github.com/ttnmapper/ttnmapper-web-v2 - A new frontend written using the React framework. The goal of the frontend is to replicate the current website feature set, but also allow users to log in using their TTN OAuth credentials. Logged in users can view and manage their own raw data.

* https://github.com/ttnmapper/ttnmapper-api-v2 - A backend for the new frontend.

# Mobile phone apps

* Android: https://github.com/ttnmapper/ttnmapper-android-v3
* iOS: https://github.com/ttnmapper/ttnmapper-ios
* Cross Platform (work in progress): https://github.com/ttnmapper/ttnmapper-flutter

# Hardware

* https://github.com/ttnmapper/ttntrackernode - Hardware and firmware for an example tracking device that can be used for coverage mapping.

* https://github.com/ttnmapper/gps-node-examples - Example firmware sketches, data encoding formats and decoders for a range of common off the shelf devices.
