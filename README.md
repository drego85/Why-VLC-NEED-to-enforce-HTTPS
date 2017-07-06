# PoC Video
https://www.youtube.com/watch?v=dhTAqCz7ktU

# Man in The Middle
VLC's official website, even having a valid SSL certificate, does not force users to use the HTTPS protocol. Major search engines have indexed VLC's website as HTTP so users are being redirected to the insecure version of the site.

Ludovic Fauvet of VideoLan denied that was possible to inject a malware in the current VLC software distribution and asked for a Proof of Concept: 
![alt text](https://raw.githubusercontent.com/drego85/Why-VLC-NEED-to-enforce-HTTPS/master/etixxx.png)

According to the official stats, published by VLC founder, the software has been downloaded over 2.3 billion times: https://twitter.com/etixxx/status/881926077836398592

That's a serious problem to be urgently worked on, because governments around the world infect with malware their citizens for spying purposes, [leading to torturing and killing in dictatorship](https://twitter.com/botherder/status/882243803448561665) when executables are being served in HTTP.

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

# Tickets to improve Browser Security 
Multiple tickets has been opened to Chrome Bug Tracker, Mozilla Firefox BugTracker and Tor Project's TorBrowser one in order to implement a feature to natively block or provide a big-red-warning (like for wrong TLS certificates) for users downloading executable code over HTTP in clear text.

That would likely obbly any software distribution to happen over an encrypted and authenticated channel.

Below the issues, please support them:

[Firefox: Warn when downloading executable files over HTTP](https://bugzilla.mozilla.org/show_bug.cgi?id=1303739)

[Chromne: Downloading windows executable over HTTP in clear-text should provide a red warning](https://twitter.com/fpietrosanti/status/882146954276483072)

[Tor Browser: Tor Browser does not provide red security warning for downloading executable in HTTP](https://trac.torproject.org/projects/tor/ticket/22809)


# Tickets to improve VideoLan
Multiple tickets to fix the situation, also considering the VideoLan project constrain and resources, has been opened but unfortunately there was not acknowledgement of the various sustainability proposals to be full-HTTPS.

All of those tickets has been closed as invalid or duplicate after a brief discussion denying the possibility to do it or declaring totally invalid the solutions proposed to achieve a full-HTTPS distribution of software, including threatening of being banned out of the ticketing system.

[Enforce HTTPS for distribution of software as a security measure against targeted malware attacks](https://github.com/etix/mirrorbits/issues/59)

[Enforce the uses of HTTPS for all websites of VLC/videolan.org to prevent MITM exploits](https://trac.videolan.org/vlc/ticket/18472)

[Map all the existing VLC mirrors that already support HTTPS](https://trac.videolan.org/vlc/ticket/18484)

[Mirrors on fast HTTPS content delivery networks](https://trac.videolan.org/vlc/ticket/18491)

[Introduce Mirrors of VLC with Mirrors from SourceForge for VLC](https://trac.videolan.org/vlc/ticket/18492)

[Integrate Cloudflare free as an HTTPS domain fronting and caching for www.videolan.org and get.videolan.org](https://trac.videolan.org/vlc/ticket/18486)

[A solution to provide secure downloads using existing mirrors](https://trac.videolan.org/vlc/ticket/18489)


# Credits
* Fabio (Naif) Pietrosanti 
* Andrea (Drego) Draghetti
* Eddy (mane) Boscolo

# Disclaimer 
All of provided information in this article is for educational purposes only. You accept full responsibility for the consequences of your use, or non-use, of any information provided by us through any means whatsoever.
