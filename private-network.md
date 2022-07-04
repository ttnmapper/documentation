# Mapper for private networks

The TTN Mapper system can be used to map the coverage of private LoRaWAN networks. Multiple options exist for this.

## The Things Stack

The most The Things Stack instances exchange message via the Packet Broker. 
TTN Mapper is able to detect the forwarding and the home networks for these messages.
For networks like these a cloud offering is available which includes a white labelled map. 
The map is hosted under &lt;name&gt;.coveragemap.net, but an alias on your company domain can be set up.
This solution starts at $20 per month, increasing with the number of gateways on your network.
For the latest pricing details, please have a look at the description on Patreon: https://www.patreon.com/ttnmapper

## ChirpStack
  
Some Chirpstack instances exchange data via the Packet Broker. For networks like these a SaaS solution like we have for The Things Stack is ideal.

For free standing network servers a hosted solution is available which includes support.
You can also deploy your own instance by using the open source code from Github, but this comes without any support.

## Helium

The Helium network is currently seen as one big network server, and the coverage is available under https://coveragemap.net. 
When Helium matures and it becomes clear how private networks will operate, white labelled maps will be made available.

## Other

It is possible to use the Mapper technology for any LoRaWAN network, or any wireless network for that matter. 
If you have a requirement for a coveragemap, please contact us and we can do a feasibility study and provide a quote.

## Contact Us

{::nomarkdown}
<!-- modify this form HTML and place wherever you want your form -->
<form
  action="https://formspree.io/f/xyyoqzwl"
  method="POST"
>
  <label>
    Your email:
    <input type="email" name="email">
  </label>
  <label>
    Your message:
    <textarea name="message"></textarea>
  </label>
  <!-- your other form fields go here -->
  <button type="submit">Send</button>
</form>
{:/nomarkdown}
