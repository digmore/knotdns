# Authoritative KnotDNS Docker image

# What is this?

KnotDNS is an authoritative name server for the domain name system.

This image exists because I wanted a Knot container based solely on Debian
packages so I could track patching through Debian Security Advisories.

You can get started using this container with as little as the zone
configuration for your domain.

# How to use this image

The default configuration in this image expects a zone configuration to be
provided using a volume mounted to `/etc/knot/zones`

At a minimum there should be a file `/etc/knot/zones/zones.conf` with a
configuration for all of your zones such as:
```
example.org {
  file "/etc/knot/zones/example.org.zone";
}
```
and an RFC 1035 format zone file (e.g. Bind "text" format) for each zone such
as:
```
$ORIGIN example.com.
$TTL 3600

@       SOA     dns1.example.com. hostmaster.example.com. (
                2010111213      ; serial
                6h              ; refresh
                1h              ; retry
                1w              ; expire
                1d )            ; minimum

        NS      dns1
        NS      dns2
        MX      10 mail

dns1    A       192.0.2.1
        AAAA    2001:DB8::1

dns2    A       192.0.2.2
        AAAA    2001:DB8::2

mail    A       192.0.2.3
        AAAA    2001:DB8::3
```
Launching the knotdns container on a single public address:

    docker run -d --name=knotdns -p 198.51.100.8:53:53 -p198.51.100.8:53:53/udp -v /opt/knotzones:/etc/knot/zones:ro digmore/knotdns
    
You should swap out `198.51.100.8` for your own IP address of course.

# Security updates
All software within this image is from the Debian project, so you can
get notification of vulnerabilities through the [Debian
Security](https://www.debian.org/security/) team's announcement mailing list as
they become aware of them.

This image is linked against the debian:jessie official image. As fixes
become available in that base image a new build of this one should be created
automatically. I will do my best to trigger new builds if there is an update
to knotdns itself through Debian, but make no guarantees. Please contact
me through GitHub if you notice a fix is not yet incorporated.

Before putting this on an untrusted network you should read and understand the
configuration, and ensure that it meets your requirements.

# User Feedback

## Issues
If you want to contact me with questions, or to report problems with this
image, please raise a [GitHub issue](https://github.com/digmore/knotdns/issues)


