# Debugging LAN Discovery

## Introduction to MDNS

The Tessel 2 CLI is able to detect Tessel 2s on the same wireless network as your host computer using a protocol called [mDNS](https://en.wikipedia.org/wiki/Multicast_DNS). If you run `t2 list` and don't see your Tessel labeled LAN like the example below, then either the Tessel is not advertising properly, your wifi network infrastructure blocks mDNS traffic, or your computer is not picking it up properly.

```
➜  t2 list
INFO Searching for nearby Tessels...
  Frank LAN
```

## An issue with detection
First, you should confirm your host machine is on the same wifi network as your Tessel. You can run `t2 wifi` to get the network your Tessel 2 is connected to. If it's not connected, you'll need to do that and run `t2 list` again.

Next, let's check if your computer is able to detect the Tessel at all (to determine if it's an issue with the CLI or with the Tessel). We'll use a utility that we absolutely know works.

*I only have an example for OSX - if you know how to do mDNS discovery on Windows or Linux, please add to this file and submit a PR!*
### OSX
```
dns-sd -B _tessel._tcp .
```
This tells the `dns-sd` (DNS Service Discovery) tool to start searching for any devices advertising as a `_tessel._tcp` device. If your device doesn't show up in the results, then it is an issue with detection, then your computer isn't detecting it up properly.

There is also issue a peculiar but uncommon issue where OSX just stops picking up the advertisements for some reason. The cause is unknown thus far but you can fix it with:
```
sudo killall -HUP mDNSResponder
```
This command basically clears the cache of mDNS devices so the next time you run the `dns-sd` utility or `t2 list`, Tessel will show up (if this was the root cause).

## An issue with network configuration
Attempts to connect to your Tessel 2 via a network that is configured to block mDNS traffic will always fail. This type of configuration is often used for secured company networks and open public networks to prevent unwanted peer-to-peer discovery and access. If this is your home network, change the configuration via the router's administration control panel. If this is a network that you do not control, you may have to request the change from an IT department (or similar resource).

## An issue with advertisement
To determine if it's an issue with Tessel not advertising properly, you'll need to get [root access](/Root_Access.html) to your Tessel. Then run `ps` to check if the mDNS service is actually running on Tessel:
```
 1259 root       904 S    dns-sd -R Frank _tessel._tcp local 22
```
If you don't see this service running (note that the name of your Tessel will be different), you can restart it with:
```
/etc/init.d/tessel-mdns restart
```
Once it's restarted, check if it's running with the `dns-sd` utility on your host machine or `t2 list`.
