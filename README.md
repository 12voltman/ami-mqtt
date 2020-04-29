# ami-mqtt
Asterisk AMI to MQTT Gateway

Uses Asterisk AMI to read event stream from Asterisk instance and generate
status messages to forward to MQTT broker.

Events tracked:
* Incoming calls from trunks ring -> answer -> hangup
* Outgoing calls on trunks from extensions
* Extension activity for incoming, outgoing and extension to extension calls

Given the many ways Asterisk can be configured, extensions, trunks and external numbers
are determined by pattern matching numbers. The patterns are defined in config.json.

Asterisk context information is ignored.

Note that calls to pseudo phone extensions can be used to generate automation events.

Status information is determined heuristically. Your mileage will vary.

Requires a config file - config.json - See config.json.sample

Remember that comments will need to be removed and JSON is extremely picky
about formatting.

## Installation Steps

1. Choose installation location and run ```git clone ___LINK_HERE____``` (for RasPBX run this command in the /root folder, which is where you default to on ssh login)

2. Edit ami-mqtt.service as follows (for RasPBX skip this step)
   * Set "User" equals to the user you want the service to run as
   * Set "Group" equals to the user group that the chosen user is in
   * Set "WorkingDirectory" equals to the location you installed to in step 1
   * Save and exit your editor
   
3. Verify that the following NPM modules are installed, or install using ```npm install __package-name___```
   * util
   * nami
   * mqtt
   
4. Make a file named config.json following the format of config.json.sample, being sure to remove all the comments while maintaining proper syntax (I recommend using an online json parser to verify your final file before continuing)

5. Run the following commands to start the service
   * ```sudo cp ami-mqtt.service /etc/systemd/system``` Copies the service file you edited to the proper location
   * ```sudo systemctl start ami-mqtt.service``` Starts the service for this session
   * ```sudo systemctl enable ami-mqtt.service``` Ensures service starts after boot in the future
