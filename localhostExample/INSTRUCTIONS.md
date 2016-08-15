this is based on the java webrtc player example to test that your setup works.
 
you need to have gstreamer and kurento installed and the media server running, with this command:
cd /tmp
sudo service kurento-media-server-6.0 start

make the FIFO for it to route audio into (you could do netcat instead but on the same machine this is simplest)
 
mkfifo audioFIFO.wav

this works like a named pipe but shows up as a wav file to kurento

play something that makes noise (random youtube video or whatever)

pactl list | grep -A2 'Source #' | grep 'Name: .*\.monitor$' | cut -d" " -f2


returns a list of monitor sources on your machine, run this with your monitor source instead of what my machine has:

gst-launch-1.0 pulsesrc device = "alsa_output.pci-0000_00_1b.0.analog-stereo.monitor" ! wavenc ! filesink location="audioFIFO.wav"


launch this java example with:

mvn compile exec:java

and assuming the java gods smile upon you..

go to https://localhost:8443/

click to add an exception because my certificates are all self signed nonsense right now

a UI should appear, select audio only and paste in:

file:///tmp/audioFIFO.wav

and hit play.

If things are going correctly you should hear an ungodly mess because the fifo is full of audio from when gstreamer started running output to it along with the live feed of what you
have playing. You can hit pause and it will stop and then hit play and it will start back. that means things are all hooked up and streaming through kms.



