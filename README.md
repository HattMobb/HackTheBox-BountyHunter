# HackTheBox-BountyHunter
A walkthrough/ write-up of the "BountyHunter" box following the CREST pentesting pathway feautring XML injection, code analysis, and web vulnerability assessment.

## Enumeration

First scan ports reveales an Apache web server:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/973d4690-715b-4ead-82d4-bd782eab1490)


Gobuster scan of the site:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/7f3e51c4-3f4c-4790-806d-49f818827af2)

Beta form page left on the site:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/80441fcc-a855-470b-b5e8-4f37efec261d)

Filling in the form directs to:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/6d1c02df-435a-4f2f-b283-825259d3e3b9)

`data` payload is encoded as URL > Base64 which gives the original input:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/9e4d486b-6da3-4f6f-babb-6f30872f4455)

Tried XXE exploit using 
```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE data [
<!ENTITY file SYSTEM "file:///etc/passwd"> ]>
```

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/efa360b2-cd68-4d2c-8f7f-1d728a3a689d)

Knowing I could take advantage of this, I played around with trying to reach the server source code, again encoding the XXE in Base64 > URL:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/92471984-905e-4afb-b213-7aac152f3299)


PHP database file contains credentials:

![image](https://github.com/HattMobb/HackTheBox-BountyHunter/assets/134090089/6bff1e36-061c-429e-a461-fbe058667840)


