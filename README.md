== oscreplayer - <em>utility to replay OSC messages</em>
originally by {Tom Lieber}[https://github.com/alltom] with modifications by {Mark Cerqueira}[https://github.com/markcerqueira], {Spencer Salazar}[https://github.com/spencersalazar]

===HOW TO
====SETUP
Run *bundle* *install* to get the required gems. We developed this on Ruby version 1.9.3 so you should use at least that. Run <b>ruby --version</b> to check your version of ruby. 

====RECORDING MESSAGES
Recording OSC messages with *oscrecorder* requires specifying the port to listen for OSC messages on and optionally, specifying a filename to write YAML-encoded message data to. If no filename is specified, data is written to stdout. Message data is cached and flushed to the file/stdout every 2 seconds and when an interrupt (i.e. SystemExit, Interrupt) is received. Be careful to only send one interrupt (crtl+c only once) because you may interrupt the last flushing of data if you send multiple interrupts.
 
   # grabs OSC messages received on port 4420, writing data to the osc_data.yml file
   $ oscrecorder 4420 osc_data.yml

   # grabs OSC messages received on port 6600, writing data to stdout; here we pipe that data into 
   # a hypothetical script, osc_processor, that does real-time processing
   $ oscrecorder 6600 > osc_processor

====PLAYING BACK MESSAGES
Playing back OSC messages with *oscplayer* requires specifying the address and port to send OSC messages to and optionally, a boolean to specify whether the padding for the initial message (the time from when recording began to when the first message was received) should be respected, and a filename to read data from. If no boolean is specified, the initial time padding is ignored. If no filename is specified, oscrecorder will attempt to load data from stdin.

   # parses messages from osc_data.yml, sending them to 127.0.0.1:5200 and respects the initial padding
   # on the first message
   $ oscplayer 127.0.0.1 5200 true osc_data.yml

   # parses messages from osc_data.yml (read via stdin), sending them to 127.0.0.1:5400 and ignores the 
   # initial padding on the first message (i.e. starts sending messages immediately)
   $ osc_data.yml | oscplayer 127.0.0.1 5400 

===MY DESIRES
1. Better options parsing (maybe with trollop)
2. Utility to display data of an encoded file
3. GUI a la {Charles Proxy}[http://www.charlesproxy.com/] that allows for real-time recording and playing back with data modifications.

===FAQ
1. Help! When I run *oscrecorder* I get this error every time a message is received:
    network_packet.rb:4:in `initialize': undefined method `force_encoding' for #<String:0x10ee199e8> (NoMethodError)

Check your version of ruby. force_encoding is not implemented in Ruby 1.8.

===LICENSE
Tom originally developed this code and goes to MIT so oscreplayer is released under the {MIT License}[http://opensource.org/licenses/MIT]. 