# FAQ

## License

All data from TTN Mapper (ttnmapper.org) should be considered under the PDDL v 1.0 license (<a href="http://opendatacommons.org/licenses/pddl/1.0/">http://opendatacommons.org/licenses/pddl/1.0/</a>).

The source code for the TTN Mapper system is open source and <a href="https://github.com/ttnmapper">available on Github</a>.


## How can I contribute data?

### Using any LoRaWAN device and a smartphone

* A node transmitting to The Things Network. An Arduino with a RN2483 radio is ideal. Have a look at <a href="https://github.com/jpmeijers/RN2483-Arduino-Library">this library</a> for example code and wiring instructions for the RN2483 and Arduino. Your node can however be any hardware and run any code, as long as you know the address the device use to transmit data to The Things Network.

* An Android phone running the TTN Mapper App available <a href="https://play.google.com/store/apps/details?id=org.ttnmapper.phonesurveyor">on the playstore</a>. -OR- An iPhone running the TTN Mapper app available from the iTunes store.

Also see <a href="https://www.thethingsnetwork.org/labs/story/using-ttnmapper-on-android">this lab</a> for more details.

### Using a GPS tracker device, transmitting GPS coordinates in the LoRa payload

<a href="https://www.thethingsnetwork.org/docs/applications/ttnmapper/">See the documentation here.</a>

If you have a GPS tracker that sends its own location in the payload of the LoRa packet, it is as easy as enabling the TTN Mapper integration to contribute data to the coverage map.</p>

The assumption is that your end device with a GPS on it sends at least its latitude and longitude, but preferably also its altidude and HDOP values. If HDOP is not available, the accuracy of the GPS fix in metres will be accepted. As a last resort the satellite count can be used.

Make sure you have a Payload decoder function enabled on the TTN Console that decodes the raw payload into a json object. The JSON object should contain the keys "<b>latitude</b>", "<b>longitude</b>" and "<b>altitude</b>" and one of "<b>hdop</b>", "accuracy" or "sats". When using the Cayenne LPP data format, the GPS coordinates will be decoded into a different JSON format, but this format is also supported. Cayenne LPP does not contain a GPS accuracy, and therefore this data will be considered as inferior and will carry less weight in calculation of coverage, and will be deleted first during data cleanup.

On the TTN Console, open your application and then click on Integrations. Search for the TTN Mapper integration and click on it. 
* Process ID: fill in a unique string describing this integration. The value does not have an influence on the correct functionality of the integration.</li>
* E-mail address: fill in a valid email address. This email address will be used in the future to associate all data to you, and guarantee the quality of the data.</li>
* Port filter: This text field can be left <b>empty</b> in the most cases. If you are using your application for multiple purposes, and only send GPS coordinates on one specific port, you can use this field to specify the port on which the GPS coordinates are sent.</li>
* Experiment name: This text field can be left <b>empty</b> in the most cases. If you are measuring coverage that is out of the ordinary, like taking a GPS tracker on an <b>aeroplane</b>, strapping it to a <b>balloon</b> or <b>drone</b>, or climbing a <b>high mast</b> or <b>tower</b> that is not publicly accessible, your data should be logged to an experiment to keep it separated from the main TTN Mapper global coverage map.</li>

For more info see the following articles:
* <a href="https://www.hackster.io/Amedee/the-things-network-node-for-ttnmapper-org-a8bcd4">Hackster.io - The Things Network Node for TTNmapper.org</a></li>
* <a href="https://github.com/ricaun/esp32-ttnmapper-gps">ESP32 TTN Mapper GPS</a></li>
* <a href="https://www.thethingsnetwork.org/labs/story/payload-decoder-for-adeunis-field-test-device-ttn-mapper-integration">Adeunis Field Test device</a></li>
* <a href="https://github.com/ttnmapper/gps-node-examples">Other examples</a></li>


## When should I use an "experiment" to map my coverage</h5>
An experiment is a way to keep unrealistic coverage measurements away from the main map. Experiments should be used when coverage is mapped from aeroplanes, balloons or any similar unrealistic altitudes.

In other words logging to the main map should only be done from roughly 0.5m-2m above ground level. "Ground level" should be interpreted as any place easily accessible by a human - or any place where an IoT device would commonly be installed. The top of a skyscraper is only acceptable if the skyscraper has a viewing deck that is publicly accessible. Man made hills and natural mountains are acceptable. The roof of a car or small delivery truck is fine. The roof of a bus or 14 wheeler truck is not as that is not a average acceptable height at which a sensor will be installed. The dashboard of a truck or bus is however roughly 2m above ground and therefore acceptable.


## Which spreading factor should I transmit on?</h5>

We recommend using SF7. This provides us with a map of "worst case" coverage. Due to the 1% duty cycle limit on transmissions, using the fastest spreading factor will maximize the number of measurements taken.

From experimentation it has been seen that slower spreading factors (like SF12) does not perform well when the device is mobile. This is likely due to path fading from obstacles in the radio path, causing bursts of interference (erasures) which the forward error correction can not deal with. When you are doing measurements while standing still higher spreading factors (SF9-SF12) will work, but when mobile it's better to use SF7.


## My results are not showing up on the map</h5>

New measurements take up to 24 hours to be processed and displayed on the main map. If you want to see if your data is being uploaded, use the advanced map options and filter by your device ID.

It is possible that the gateway that received your packets does not have its location configured. If you know the gateway owner, please ask them to configure it. See "My gateway is not showing on TTN Mapper". If this is the reason for your measurements not showing, you can check under Advanced Maps, enter your node address, and view the map. If you now see circles without lines connecting them to a gateway, this issue is confirmed.

Did you mark your data as experimental? If so, the data will not show on the main map, but on a separate map containing only your experiment. Please disable experimental mode in the app so that you can contribute to the global map of TTN coverage.


## While I was measuring I saw a lot of points on the app for locations I measured, but only a few points are shown on your map. Did some of the data points go missing, or not upload?</h5>
The most likely reason is that the GPS accuracy of your phone or device was not good enough. Make sure you have a location accuracy of less than 10 meters, or an HDOP of less than 2.

Data logged to an experiment will not be displayed on the main map. Use the experiments section to display your experiment's data. Experiments have less strict location accuracy filters.

It is possible that some datapoints did not upload correctly to our server because your internet connection was intermittent. This is however unlikely as a working internet connection is needed to receive data packets from TTN.


## My results were showing on the map before, but is gone now.</h5>

When a gateway moves more than 100 meters it is seen as a new installation with different coverage. All old coverage data is hidden because the old coverage is now invalid.

In a very few circumstances obvious incorrect measurements might be deleted from the map.


## My gateway is not showing on TTN Mapper</h5>

For a gateway to appear on TTN Mapper its location needs to be known, and it needs to have at least one coverage mapping point uploaded to TTN Mapper. In other words if a gateway has not been measured yet, the gateway will not appear on TTN Mapper.

Make sure your gateway's GPS coordinates are configured correctly on the TTN Console. Also make sure fake_gps is disabled in the gateway's local_conf.json file. </p>
    
Also check that your privacy settings make the status and location of your gateway public.
    
    <img src="/resources/TTNConsolePrivacy.png" />


TTN Mapper will use the location configured on the TTN Console first. If the location is not configured there it will use the location which the gateway reports. That means that a gateway with a built in GPS will be located at the location set on the Console, not the location the gateway reports. Only if the location is not set on the Console the fallback location - the location reported by the gateway - will be used.


## Can I integrate your data into my map? <br /> Can I download my data which I uploaded to you?<br /> Public API access. </h5>

The GEOJSON files used to draw the TTN Mapper maps are publicly available at <a href="http://ttnmapper.org/geojson">http://ttnmapper.org/geojson</a>.

Data is sometimes dumped into CSV files. The dumps are available at <a href="http://ttnmapper.org/dumps">http://ttnmapper.org/dumps</a>. If you want newer data than what there are dumps of, please contact me.

No public API access is given to the raw data. This is due to the amount of data and number of possible queries that won't be possible to handle on the server. Please use the dumps and your own database.


## I want you to remove my gateway or measurement points from your map (privacy, etc)</h5>

If you change your gateway's location to another one more than 100m away, any old measurements for your gateway will be hidden automatically.

In any other circumstance, contact us.


## How can I support the project</h5>

One way of supporting the project is by contributing to the open source code on <a href="https://github.com/ttnmapper">Github</a>.

The project however has a monthly running cost which was covered by The Shuttleworth Foundation up to 2018. At this point there is no financial support from any official sources. If you want to financially support the project there are a couple of methods:

* <a href="https://www.patreon.com/ttnmapper">Patreon</a></li>
* Any Patreon tier as a support contract with commercial invoice - contact us for details</li>
* <a href="https://paypal.me/ttnmapper">Once off donation via PayPal</a></li>
* Once off donation into a European bank account (IBAN) - contact us for details</li>

For details contact us.


## My question is not answered here</h5>

* Ask your question in the #ttnmapper channel on the TTN Slack.</li>
* Send me a message to <info@ttnmapper.org>
