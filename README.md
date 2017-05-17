net-dev-kick
============

Fix a network interface if the network is down.

This script attempts to ping a server that *should* always be up (use your
router or something similar) and if it fails, it restarts the interface given
as `$1`.

This script is intended to be used on the raspberry pi to fix wlan0, but can
theoretically be used on any machine to fix any interface.

Why?
----

If you have a raspberry pi on WiFi then you probably already know the answer to
this, but if you don't, google "raspberry pi wifi hang".

If *anything* goes wrong with the WiFi (access point restarts, IP address
changes, you look at the pi the wrong way, etc.) the WiFi will drop and never
fix itself.  There are a lot of small scripts floating around forums that do
what `net-dev-kick` does (albeit, poorly and with a lot of assumptions) so I
decided to clean it all up and make a simple yet robust program to deal with this
situation.

Usage
------

```
Usage: net-dev-kick [-t <tries>] [-w <deadline>] <server|ip> <int1> [int2 ...]

Options:

  -h              print this message and exit
  -t <tries>      number of times to try the ping command, defaults to $tries
  -w <deadline>   passed to ping as -w, timeout in seconds for the ping command, defaults to $deadline

Example:

  $ net-dev-kick -t 5 -w 5 10.0.1.1 wlan0

This will attempt to ping 10.0.1.1 (presumably a router) 5 times
(maximum, -t 5) with a forced timeout on the ping command of 5 seconds
(-w 5).

If any ping is succesful, this program will exit cleanly.  If all 5 pings
fail, the interface wlan0 will be brought down and back up.
```

I have this in cron on my raspberry pi to ping my router every 5 minutes

License
-------

MIT License
