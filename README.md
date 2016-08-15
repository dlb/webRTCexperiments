# webRTCexperiments
Kurento, gstreamer, and Docker audio routing shenanigans. Sound plays on docker instance, gets piped into kurento (without having to modify the applicatin you are emulating through the magic of gstreamer) and sent out over webRTC through your web browser into your ears. Could send sound back if you would like to yell at your docker containers through your computer's mic for some reason. Supports a variety of plugins (anything gstreamer and openCV can do, it can do) and protocols for audio/video/data streaming (webRTC, RTP, web secure sockets, etc etc) for synchronizing with other servers, analytics, transcoding, augemented reality, etc etc etc. 

I'm building this as decoupled as possible in case you want a different docker container holding just kurento for security reasons or because of how you might  have  VNC stuff set up or  you just like a docker for everything and everything in its docker or whatever. It can be made more monolithic,  using kurento's built in gstreamer stuff running inside the docker container you are doing an emulation in if necessary for example. But I'm starting with loosely coupled for the initial tests until there are some measurements to figure out what actually will work best before hard coding things for optimiztion. 

I also have been doing my devolopment on a 7 year laptop because the room my main dev machine is in doesn't have AC and I would die ...but also to make certain this runs on low powered systems to keep AWS bills to a minimum. If you for some reason had machines with piles of GPU power the format trancoding could be optimized for CUDA/OpenCL but I'm not assuming that is the case.

#Stuff you will want to just apt-get/synaptic/etc 
If you don't already have the current version and you aren't using the docker containerized version. Wherever possible I'm using the version in the apt repository for the latest LTS release of Ubuntu (actually Lubuntu since my laptop in ancient, so I had to install some things that would just come with a standard Ubuntu install), currently Xenial. All of these can be built from source with by hand with make if you really want to, but it's time consuming and  not necessary.

http://www.kurento.org/ - media server/webRTC gateway/gstreamer-openCV host

https://gstreamer.freedesktop.org/ - get gstreamer 1.8.2 (current stable version) and install the base,good,bad and ugly plugins if you don't already have it installed. Make sure you can do 1.0 stuff not just .10 . 

https://nodejs.org/en/ - kurento has thrift bindings to javascript and java and supports a json-rpc api that can be called by anything with a little work. Path of least resistance is node on the server and browser javascript doing json magic on the client.I use the http server for testing and a few other things. These could all be changed to meet requirements of existing systems, this was just the quickest way to get something going. 

Use NPM to get http-server or run whatever http server you like best.

https://bower.io/ - for managing your javascript stuff in the client. get this from NPM for node.

https://www.freedesktop.org/wiki/Software/PulseAudio/ - really should already be installed with your distribution but worth checking, it simplifies identifying audio devices and generally makes things nicer.

https://www.docker.com/ - to dock your dockers

#Security Things

My test implementation is obviously not production ready (since it would need to be matched to your production environment) but even so kurento wants everything sent over secure protocols with proper certs even when running on local host. It is charmingly paranoid. The documentation on this is frustratingly vague, but I managed to sort it out. I'm going to submit a patch to their docs to make this clearer for others. Get these if you don't already have them. Or you may have existing certs that you can just use instead.

https://www.openssl.org/ - kurento's control API  won't talk to your web browser except over https. I'm using self signed certs for localhost to test on my machine, so I have to add exceptions for them. In production use whatever certs you are already using.

https://www.gnutls.org/ for Reasons it was easier for me to make the cert for kurento to send data over wss:// secure sockets using certtool. This how the webRTC goodness that you requested over https gets sent back to your web browser and into your ears.







