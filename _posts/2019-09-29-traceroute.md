---
layout:       single
title:        "Understanding Networks Week 04: traceroute"
date:         2019-09-29 11:59:49 -04:00
categories:   Understanding
toc:          true
---

# Destinations

* undernets.me
* itp.nopivnick.com
* big.net

[undernets.me](undernets.me) is the host I set up for our first assignment. It's a [virtual private server (VPS)](https://www.digitalocean.com/products/droplets/) on [DigitalOcean](https://www.digitalocean.com/)'s cloud hosting service. I opted to host in their [New York City based datacenters](http://speedtest-nyc3.digitalocean.com/). As of this writing it's not running a web service and so won't call up a website but the domain name will resolve from the commandline.

[itp.nopivnick.com](itp.nopivnick.com) is my blog for school. It's built using the [Jekyll](https://jekyllrb.com/) static site generator and hosted on [GitHub Pages](https://pages.github.com/). The domain is registered with [Gandi.net](https://www.gandi.net/) and the DNS record includes a [CNAME entry](https://docs.gandi.net/en/domain_names/faq/record_types/cname_record.html) that lets me [use a custom domain with content hosted on GitHubPages](https://help.github.com/en/articles/about-custom-domains-and-github-pages).

[big.net](big.net) is owned and administered by my cousin's husband, Sanner. My first email address after college was noah@big.net. Big.net a linux host that's been running on bare metal since 1994 and it's physically located in San Francisco. Sanner was among the first individuals to purchase a domain from [Network Solutions, Inc.](https://www.networksolutions.com/), the first registrar with [a government contract to offer domain name registration the public in the early 90's](https://www.icann.org/en/history/early-days).

# Networks

* NYU
* Verizon Fios Residential (home)
* AT&T Wireless (data over cellular) w/ Private Internet Access VPN
* NYU Shanghai (VPN)

# Tools - commandline

* ```traceroute``` (macOS)
* ```tcptraceroute``` (installed with homebrew)

Traces using the implementation of ```traceroute``` that comes with Mojave rarely reached their intended destination, which always leaves one feeling they're 'doing it wrong' so I ran four  traces per destination using the four different IP protocols (```-P UDP/TCP/GRE/ICMP```).

Given the ubiquity of the web, my assumption was TCP would yield the most interesting results but oddly *every* TCP trace I ran using ```traceroute``` from *every* network I was on timed out.

After some searching, I found a TCP-specific implementation called [```tcptraceroute```](https://github.com/mct/tcptraceroute).

From ```tcptraceroute```'s man page:

> The  problem  is  that with the widespread use of firewalls on the modern Internet, many of the packets that _traceroute_(8) sends out end up being filtered, making it impossible to completely trace the path to the destination.  However, in many cases, these firewalls will permit inbound TCP packets to  specific ports  that  hosts  sitting behind the firewall are listening for connections on.  By sending out TCP SYN packets instead of UDP or ICMP ECHO packets, tcptraceroute is able to bypass the most common firewall filters.

While looking for alternatives to macOS Mojave's ```traceroute``` I also stumbled upon [Dublin Traceroute](https://dublin-traceroute.net/). ```dublin-traceroute``` was of particular interest given it's focus on [carrier-grade NAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT). CGN complicates port forwarding and I've had some trouble in the past trying to use SSH on port 22 over cellular data networks. Unfortunately, as of this writing ```dublin-traceroute]``` [doesn't build successfully on Mojave](https://github.com/insomniacslk/dublin-traceroute/issues/62).

## Tools - visualization

* [Visual Trace Root](https://www.yougetsignal.com/tools/visual-tracert/)
* [Traceroute Mapper](https://stefansundin.github.io/traceroute-mapper/)

[Visual Trace Root](https://www.yougetsignal.com/tools/visual-tracert/) is one in a suite of web-based network tools at yougetsignal.com but it has never worked for me.

[Traceroute Mapper](https://stefansundin.github.io/traceroute-mapper/) is a sleek tool hosted on GitHub Pages. It parses ```traceroute``` output, passes the IP addresses to [ipinfo.io](https://ipinfo.io/) using their API, and plots approximate physical locations on Google Maps. It's the cleanest traceroute mapping visualizer I was able to find -- web-based or otherwise -- and it's designed to use commandline output from a trace run locally, as opposed to plotting traces that originate from the host serving the visualization tool.

# Observations

## ```traceroute``` vs ```tcptraceroute```

Using ```traceroute``` with the ```-P TCP``` flag yielded no results whatsoever on any of the three networks. This is perplexing and I wonder whether Apple's implementation either breaks or doesn't support that particular upstream feature.

Using ```tcptraceroute``` was generally more successful creating a trace. Nonetheless the target destination wasn't reached within the default 30 hops on all but a few instances.

## Cogent (and Verizon)

[Cogent](Cogentco.com)'s network appeared on many of the traces I ran. According to their wesite:

>Cogent is one of the world's largest Internet Service Providers, delivering high quality Internet, Ethernet and Colocation services to over 84,000 Enterprise and NetCentric customers. Cogent serves over 204 markets in 43 countries across its facilities-based, all-optical IP network.

When running a trace from home (Verison Fios), Cogent was the next hop once the trace left Verizon's network. Poking around a bit, it turns out the two companies have a contentious history and eventually [came to terms in 2015](https://www.cogentco.com/en/news/press-releases/714-cogent-and-verizon-enter-into-interconnection-agreement).

## Geolocation by IP

Sundin's [Traceroute Mapper announcement post on Hacker News](https://news.ycombinator.com/item?id=14290608) gives some interesting insights including a post that mentions limitations of [traditional IP geolocation databases](traditional IP geolocation databases).

## The curious case of bad.horse

Toward the bottom of the Hacker News thread was a cryptic post by user thefalcon prompting readers to [Be sure to Traceroute bad.horse](https://news.ycombinator.com/item?id=14296113)".

So I did.

```
$ traceroute bad.horse
traceroute to bad.horse (162.252.205.157), 64 hops max, 52 byte packets
 1  fios_quantum_gateway (192.168.1.1)  5.689 ms  4.838 ms  4.596 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  23.463 ms
    b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  16.421 ms  23.360 ms
 4  * * *
 5  0.et-5-0-5.br2.nyc4.alter.net (140.222.1.59)  14.889 ms
    0.et-10-1-2.br2.nyc4.alter.net (140.222.235.119)  18.778 ms  17.250 ms
 6  verizon.com.customer.alter.net (152.179.72.42)  20.833 ms  20.653 ms  19.712 ms
 7  ae-18.r00.nycmny17.us.bb.gin.ntt.net (129.250.4.207)  19.362 ms  12.389 ms  19.196 ms
 8  ce-0-6-0-3-51.r00.nycmny17.us.ce.gin.ntt.net (129.250.194.106)  20.944 ms  21.327 ms  20.649 ms
 9  e8-7.cr2.lga3.atlanticmetro.net (205.251.126.43)  20.004 ms  28.440 ms  13.836 ms
10  e8-8.cr1.lga12.atlanticmetro.net (108.60.138.53)  24.107 ms  20.305 ms  23.107 ms
11  e2-19.cr2.lga11.atlanticmetro.net (69.9.34.106)  16.783 ms  22.274 ms  19.764 ms
12  sandwichnet.dmarc.lga11.atlanticmetro.net (208.68.168.214)  20.139 ms  11.699 ms  22.018 ms
13  * * *
14  * * *
15  * * *
.
.
.
23  * * *
24  * * *
25  * * *
26  * * bad.horse (162.252.205.143)  86.944 ms
27  bad.horse (162.252.205.144)  82.903 ms  88.886 ms  109.905 ms
28  bad.horse (162.252.205.145)  91.678 ms  87.673 ms  95.325 ms
29  he-s.bad (162.252.205.146)  110.826 ms  95.914 ms  98.899 ms
30  the.evil.league.of.evil (162.252.205.147)  103.094 ms  104.193 ms  96.389 ms
31  is.watching.so.beware (162.252.205.148)  105.146 ms  108.734 ms  108.211 ms
32  the.grade.that.you.receive (162.252.205.149)  197.524 ms  117.304 ms  255.074 ms
33  will.be.your.last.we.swear (162.252.205.150)  114.661 ms  113.249 ms  118.365 ms
34  so.make.the.bad.horse.gleeful (162.252.205.151)  177.334 ms  171.347 ms  124.016 ms
35  or.he-ll.make.you.his.mare (162.252.205.152)  179.601 ms  184.350 ms  199.860 ms
36  o_o (162.252.205.153)  204.169 ms  170.944 ms  184.292 ms
37  you-re.saddled.up (162.252.205.154)  219.716 ms  132.242 ms  240.595 ms
38  there-s.no.recourse (162.252.205.155)  204.698 ms  180.903 ms  205.336 ms
39  it-s.hi-ho.silver (162.252.205.156)  204.859 ms  204.627 ms  157.955 ms
40  signed.bad.horse (162.252.205.157)  160.044 ms  263.305 ms  204.178 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/bad.horse_traceroute_fios.png)

It turns out the trace in combination with (I'm guessing OpenDNS?) domain name resolutions are lyrics to the [Bad Horse Chorus](https://youtu.be/EAxQxFXSEKw?t=16) from Josh Whedon's [Dr. Horrible's Sing-Along Blog](http://drhorrible.com/).

> Bad Horse, Bad Horse  
> Bad Horse, he’s bad  
> The evil league of evil is watching so beware  
> The grade that you receive’ll be your last, we swear  
> So make the bad horse gleeful, or he’ll make you his mare  
> You’re saddled up; there’s no recourse  
> It’s “hi-yo, silver!”  
> Signed: Bad Horse.  

Curious to know more, I ran a ```whois``` on the domain bad.horse. The domain is registered with my registrar Gandi.net but the registrant opted to mask their contact information:

```
Domain Name: BAD.HORSE
Registry Domain ID: 55689_MMd1-HORSE
Registrar WHOIS Server:
Registrar URL:
Updated Date: 2019-07-17T16:43:06Z
Creation Date: 2014-09-15T16:00:14Z
Registry Expiry Date: 2020-09-15T16:00:14Z
Registrar: Gandi SAS
Registrar IANA ID: 81
Registrar Abuse Contact Email: abuse@support.gandi.net
Registrar Abuse Contact Phone: +33.170377661
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: REDACTED FOR PRIVACY
Registrant Name: REDACTED FOR PRIVACY
Registrant Organization: REDACTED FOR PRIVACY
Registrant Street: REDACTED FOR PRIVACY
Registrant Street: REDACTED FOR PRIVACY
Registrant Street: REDACTED FOR PRIVACY
Registrant City: REDACTED FOR PRIVACY
Registrant State/Province: MN
Registrant Postal Code: REDACTED FOR PRIVACY
Registrant Country: US
Registrant Phone: REDACTED FOR PRIVACY
Registrant Fax: REDACTED FOR PRIVACY
```

Undeterred, I ran a ```whois``` using the IP address from the trace which indicated the domain was associated with an IP range allocation:

```
NetRange:       162.252.204.0 - 162.252.207.255
CIDR:           162.252.204.0/22
NetName:        SANDWICH-NET-1
NetHandle:      NET-162-252-204-0-1
Parent:         NET162 (NET-162-0-0-0-0)
NetType:        Direct Allocation
OriginAS:       AS62512
Organization:   Sandwich.Net, LLC (SL-104)
RegDate:        2014-02-04
Updated:        2015-01-31
Comment:        We strive to be good Internet neighbors. Please don't hesitate to e-mail or call us about abuse and operations issues.
Ref:            https://rdap.arin.net/registry/ip/162.252.204.0


OrgName:        Sandwich.Net, LLC
OrgId:          SL-104
Address:        PO Box 583097
City:           Minneapolis
StateProv:      MN
PostalCode:     55458-3097
Country:        US
RegDate:        2012-03-16
Updated:        2017-01-28
Ref:            https://rdap.arin.net/registry/entity/SL-104
```

[Sandwich.net](https://sandwich.net) is a web hosting and consulting company out of Minneapolis, MN. They describe themselves as "Fiercely independent", which seems appropriate.

# Traces

## undernets.me via NYU

### ```traceroute```

```
$ traceroute undernets.me
traceroute to undernets.me (157.230.179.212), 64 hops max, 52 byte packets
 1  10.18.0.2 (10.18.0.2)  4.751 ms  1.936 ms  2.331 ms
 2  coregwd-te7-8-vl901-wlangwc-7e12.net.nyu.edu (10.254.8.44)  4.005 ms  2.339 ms  2.124 ms
 3  128.122.1.4 (128.122.1.4)  2.122 ms  1.739 ms  1.501 ms
 4  ngfw-palo-vl1500.net.nyu.edu (192.168.184.228)  2.292 ms  2.535 ms  2.256 ms
 5  nyugwa-outside-ngfw-vl3080.net.nyu.edu (128.122.254.114)  2.360 ms  2.183 ms  2.157 ms
 6  nyunata-vl1000.net.nyu.edu (192.168.184.221)  2.383 ms  2.526 ms  2.454 ms
 7  nyugwa-vl1001.net.nyu.edu (192.76.177.202)  2.411 ms  2.783 ms  3.159 ms
 8  dmzgwb-ptp-nyugwa-vl3082.net.nyu.edu (128.122.254.111)  3.186 ms  2.945 ms  3.084 ms
 9  128.122.254.70 (128.122.254.70)  2.821 ms  2.671 ms  2.845 ms
10  ix-xe-7-3-2-0.tcore2.nw8-new-york.as6453.net (64.86.62.13)  3.198 ms  3.718 ms  4.018 ms
11  if-ae-3-2.tcore4.njy-newark.as6453.net (64.86.62.26)  5.055 ms  5.747 ms  4.489 ms
12  if-ae-11-14.tcore2.nto-new-york.as6453.net (63.243.186.5)  4.540 ms
    if-ae-11-15.tcore2.nto-new-york.as6453.net (63.243.216.5)  4.635 ms
    if-ae-14-2.tcore2.nto-new-york.as6453.net (63.243.186.16)  5.397 ms
13  if-ae-12-2.tcore1.n75-new-york.as6453.net (66.110.96.5)  4.110 ms  4.300 ms  4.305 ms
14  66.110.96.22 (66.110.96.22)  6.307 ms
    66.110.96.26 (66.110.96.26)  9.221 ms
    66.110.96.22 (66.110.96.22)  4.463 ms
15  * * 138.197.244.10 (138.197.244.10)  6.228 ms
16  * * *
17  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute undernets.me
Selected device en0, address 10.18.27.230, port 63361 for outgoing packets
Tracing the path to undernets.me (157.230.179.212) on TCP port 80 (http), 30 hops max 1  10.18.0.2  2.647 ms  1.391 ms  1.242 ms
 2  10.254.8.44  1.366 ms  1.267 ms  1.391 ms
 3  128.122.1.4  1.451 ms  1.486 ms  1.544 ms
 4  192.168.184.228  3.078 ms  2.692 ms  1.927 ms
 5  nyugwa-outside-ngfw-vl3080.net.nyu.edu (128.122.254.114)  1.726 ms  1.610 ms  1.617 ms
 6  192.168.184.221  1.910 ms  1.808 ms  1.864 ms
 7  nyugwa-vl1001.net.nyu.edu (192.76.177.202)  1.988 ms  1.653 ms  2.032 ms
 8  dmzgwb-ptp-nyugwa-vl3082.net.nyu.edu (128.122.254.111)  2.752 ms  2.698 ms  2.355 ms
 9  128.122.254.70  2.122 ms  2.342 ms  2.173 ms
10  ix-xe-7-3-2-0.tcore2.nw8-new-york.as6453.net (64.86.62.13)  7.182 ms  11.427 ms  18.235 ms
11  if-ae-3-2.tcore4.njy-newark.as6453.net (64.86.62.26)  19.509 ms  24.055 ms  30.075 ms
12  if-ae-11-15.tcore2.nto-new-york.as6453.net (63.243.216.5)  6.615 ms  4.972 ms  3.972 ms
13  if-ae-12-2.tcore1.n75-new-york.as6453.net (66.110.96.5)  4.171 ms  3.616 ms  3.721 ms
14  66.110.96.26  2.917 ms  2.940 ms  3.235 ms
15  * * *
16  * * *
17  * * *
.
.
.
28  * * *
29  * * *
30  * * *
Destination not reached
```


## itp.nopivnick.com via NYU

### ```traceroute```

```
$ traceroute itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.110.153
traceroute to nopivnick.github.io (185.199.110.153), 64 hops max, 52 byte packets
 1  10.18.0.2 (10.18.0.2)  2.081 ms  1.452 ms  1.413 ms
 2  coregwc-te7-8-vl901-wlangwc-7e12.net.nyu.edu (10.254.6.44)  1.440 ms  1.722 ms  1.832 ms
 3  128.122.1.4 (128.122.1.4)  1.703 ms  1.645 ms  1.797 ms
 4  ngfw-palo-vl1500.net.nyu.edu (192.168.184.228)  2.210 ms  3.019 ms  2.213 ms
 5  nyugwa-outside-ngfw-vl3080.net.nyu.edu (128.122.254.114)  2.876 ms  2.228 ms  2.265 ms
 6  nyunata-vl1000.net.nyu.edu (192.168.184.221)  2.411 ms  2.174 ms  2.454 ms
 7  nyugwa-vl1001.net.nyu.edu (192.76.177.202)  3.529 ms  2.808 ms  2.540 ms
 8  dmzgwb-ptp-nyugwa-vl3082.net.nyu.edu (128.122.254.111)  3.257 ms  2.934 ms  3.002 ms
 9  128.122.254.74 (128.122.254.74)  3.542 ms  3.090 ms  2.579 ms
10  6-1-30.ear3.newyork1.level3.net (4.28.130.117)  3.874 ms  3.903 ms  3.274 ms
11  * * *
12  * * *
13  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute itp.nopivnick.com
Selected device en0, address 10.18.27.230, port 63363 for outgoing packets
Tracing the path to itp.nopivnick.com (185.199.110.153) on TCP port 80 (http), 30 hops max
 1  10.18.0.2  2.953 ms  1.323 ms  1.318 ms
 2  10.254.6.44  1.436 ms  1.359 ms  1.372 ms
 3  128.122.1.4  1.590 ms  1.563 ms  1.820 ms
 4  192.168.184.228  2.141 ms  1.669 ms  1.623 ms
 5  nyugwa-outside-ngfw-vl3080.net.nyu.edu (128.122.254.114)  1.570 ms  1.854 ms  1.493 ms
 6  192.168.184.221  1.748 ms  2.402 ms  1.999 ms
 7  nyugwa-vl1001.net.nyu.edu (192.76.177.202)  1.977 ms  2.742 ms  1.875 ms
 8  dmzgwb-ptp-nyugwa-vl3082.net.nyu.edu (128.122.254.111)  2.411 ms  3.041 ms  2.650 ms
 9  128.122.254.74  2.037 ms  2.134 ms  3.206 ms
10  6-1-30.ear3.newyork1.level3.net (4.28.130.117)  2.620 ms  2.664 ms  2.466 ms
11  * * *
12  4.30.176.218  3.848 ms  2.478 ms  2.324 ms
13  185.199.110.153 [open]  3.070 ms * 2.548 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/itp.nopivnick.com_tcptraceroute_nyu.png)

## big.net via NYU

### ```traceroute```

```
$ traceroute big.net
traceroute to big.net (157.131.153.126), 64 hops max, 52 byte packets
 1  10.18.0.2 (10.18.0.2)  1.744 ms  1.449 ms  2.310 ms
 2  coregwd-te7-8-vl901-wlangwc-7e12.net.nyu.edu (10.254.8.44)  132.168 ms  83.802 ms  18.532 ms
 3  128.122.1.36 (128.122.1.36)  2.447 ms  2.178 ms  2.169 ms
 4  ngfw-palo-vl1500.net.nyu.edu (192.168.184.228)  3.097 ms  2.863 ms  3.015 ms
 5  nyugwa-outside-ngfw-vl3080.net.nyu.edu (128.122.254.114)  2.497 ms  2.256 ms  3.230 ms
 6  nyunata-vl1000.net.nyu.edu (192.168.184.221)  4.235 ms  2.969 ms  2.968 ms
 7  nyugwa-vl1001.net.nyu.edu (192.76.177.202)  4.265 ms  19.479 ms  61.564 ms
 8  dmzgwb-ptp-nyugwa-vl3082.net.nyu.edu (128.122.254.111)  3.823 ms  4.478 ms  4.207 ms
 9  128.122.254.70 (128.122.254.70)  4.732 ms  3.548 ms  3.026 ms
10  ix-xe-7-3-2-0.tcore2.nw8-new-york.as6453.net (64.86.62.13)  3.029 ms  4.151 ms  3.020 ms
11  if-ae-3-2.tcore4.njy-newark.as6453.net (64.86.62.26)  5.851 ms  5.294 ms  5.399 ms
12  if-ae-14-2.tcore2.nto-new-york.as6453.net (63.243.186.16)  4.326 ms  4.799 ms
    if-ae-11-14.tcore2.nto-new-york.as6453.net (63.243.186.5)  98.656 ms
13  be3011.ccr31.jfk05.atlas.cogentco.com (154.54.12.17)  5.558 ms  4.689 ms  5.301 ms
14  be3294.ccr41.jfk02.atlas.cogentco.com (154.54.47.217)  7.704 ms  5.108 ms  4.802 ms
15  be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  18.921 ms
    be2889.ccr21.cle04.atlas.cogentco.com (154.54.47.49)  20.064 ms
    be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  18.867 ms
16  be2717.ccr41.ord01.atlas.cogentco.com (154.54.6.221)  23.116 ms
    be2718.ccr42.ord01.atlas.cogentco.com (154.54.7.129)  23.215 ms
    be2717.ccr41.ord01.atlas.cogentco.com (154.54.6.221)  22.957 ms
17  be2832.ccr22.mci01.atlas.cogentco.com (154.54.44.169)  56.415 ms
    be2831.ccr21.mci01.atlas.cogentco.com (154.54.42.165)  47.248 ms  46.172 ms
18  be3036.ccr22.den01.atlas.cogentco.com (154.54.31.89)  59.112 ms  57.693 ms
    be3035.ccr21.den01.atlas.cogentco.com (154.54.5.89)  59.014 ms
19  be3037.ccr21.slc01.atlas.cogentco.com (154.54.41.145)  72.017 ms
    be3038.ccr32.slc01.atlas.cogentco.com (154.54.42.97)  72.566 ms  71.634 ms
20  be3109.ccr21.sfo01.atlas.cogentco.com (154.54.44.137)  74.489 ms
    be3110.ccr22.sfo01.atlas.cogentco.com (154.54.44.141)  72.376 ms
    be3109.ccr21.sfo01.atlas.cogentco.com (154.54.44.137)  72.119 ms
21  be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  71.320 ms
    be2015.ccr31.sjc04.atlas.cogentco.com (154.54.7.174)  72.894 ms
    be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  71.337 ms
22  38.104.141.82 (38.104.141.82)  135.574 ms  72.070 ms  71.817 ms
23  102.ae1.cr1.pao1.sonic.net (70.36.205.5)  406.225 ms  467.371 ms  397.070 ms
24  * 0.ae0.cr1.colaca01.sonic.net (70.36.205.62)  81.457 ms  74.497 ms
25  0.ae2.cr2.colaca01.sonic.net (157.131.209.66)  115.879 ms  210.236 ms  110.419 ms
26  0.ae0.cr2.snfcca05.sonic.net (198.27.244.58)  93.702 ms  90.832 ms  91.107 ms
27  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  73.789 ms  73.254 ms  73.352 ms
28  * * *
29  * * *
30  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute big.net
Selected device en0, address 10.18.27.230, port 63366 for outgoing packets
Tracing the path to big.net (157.131.153.126) on TCP port 80 (http), 30 hops max
 1  10.18.0.2  72.413 ms  2.108 ms  41.794 ms
 2  10.254.8.44  2.537 ms  1.967 ms  1.371 ms
 3  128.122.1.36  1.438 ms  1.895 ms  1.901 ms
 4  192.168.184.228  3.037 ms  1.819 ms  1.853 ms
 5  nyugwa-outside-ngfw-vl3080.net.nyu.edu (128.122.254.114)  2.614 ms  2.029 ms  1.638 ms
 6  192.168.184.221  1.854 ms  2.461 ms  1.771 ms
 7  nyugwa-vl1001.net.nyu.edu (192.76.177.202)  1.661 ms  1.836 ms  1.692 ms
 8  dmzgwb-ptp-nyugwa-vl3082.net.nyu.edu (128.122.254.111)  2.216 ms  2.435 ms  2.328 ms
 9  128.122.254.70  2.243 ms  2.126 ms  2.042 ms
10  ix-xe-7-3-2-0.tcore2.nw8-new-york.as6453.net (64.86.62.13)  2.202 ms  2.760 ms  2.482 ms
11  if-ae-3-2.tcore4.njy-newark.as6453.net (64.86.62.26)  3.942 ms  3.539 ms  4.218 ms
12  if-ae-11-14.tcore2.nto-new-york.as6453.net (63.243.186.5)  4.914 ms  3.865 ms  3.731 ms
13  be3011.ccr31.jfk05.atlas.cogentco.com (154.54.12.17)  3.777 ms  3.968 ms  3.761 ms
14  be3294.ccr41.jfk02.atlas.cogentco.com (154.54.47.217)  3.600 ms  3.525 ms  4.174 ms
15  be2889.ccr21.cle04.atlas.cogentco.com (154.54.47.49)  18.563 ms  18.768 ms  18.401 ms
16  be2717.ccr41.ord01.atlas.cogentco.com (154.54.6.221)  22.331 ms  21.856 ms  21.998 ms
17  be2831.ccr21.mci01.atlas.cogentco.com (154.54.42.165)  42.988 ms  42.901 ms  48.627 ms
18  be3035.ccr21.den01.atlas.cogentco.com (154.54.5.89)  54.561 ms  55.161 ms  54.375 ms
19  be3037.ccr21.slc01.atlas.cogentco.com (154.54.41.145)  71.178 ms  71.094 ms  72.207 ms
20  be3109.ccr21.sfo01.atlas.cogentco.com (154.54.44.137)  71.696 ms  71.339 ms  72.123 ms
21  be2015.ccr31.sjc04.atlas.cogentco.com (154.54.7.174)  71.165 ms  71.379 ms  73.233 ms
22  38.104.141.82  122.044 ms  92.148 ms  76.900 ms
23  * * *
24  * * *
25  * * *
26  * * *
27  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  76.447 ms  72.780 ms  72.627 ms
28  157-131-153-126.fiber.dynamic.sonic.net (157.131.153.126) [open]  73.461 ms * 75.025 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/big.net_tcptraceroute_nyu.png)

## undernets.me via Verizon Fios

### ```GRE```

```
$ traceroute -P GRE undernets.me
 1  fios_quantum_gateway (192.168.1.1)  6.499 ms  4.975 ms  8.536 ms
 2  * * *
 3  b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  19.308 ms
    b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  24.655 ms  14.711 ms
 4  * * *
 5  0.et-5-0-5.br2.nyc4.alter.net (140.222.1.59)  37.336 ms
    0.et-10-1-2.br2.nyc4.alter.net (140.222.235.119)  15.916 ms  36.091 ms
 6  verizon.com.customer.alter.net (152.179.120.230)  19.852 ms  18.069 ms  20.863 ms
 7  if-ae-12-2.tcore1.n75-new-york.as6453.net (66.110.96.5)  18.594 ms  19.038 ms  19.042 ms
 8  * * *
 9  * * *
10  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP undernets.me
1  fios_quantum_gateway (192.168.1.1)  19.461 ms  5.404 ms  6.119 ms
2  157.230.179.212 (157.230.179.212)  37.835 ms  27.889 ms  18.637 ms
```

### ```TCP```

```
$ traceroute -P TCP undernets.me
1  * * *
2  * * *
3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP undernets.me
1  fios_quantum_gateway (192.168.1.1)  4.746 ms  4.090 ms  4.204 ms
2  * * *
3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  23.850 ms
   b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  18.559 ms
   b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  21.175 ms
4  * * *
5  0.et-5-0-5.br2.nyc4.alter.net (140.222.1.59)  16.127 ms  18.904 ms  24.357 ms
6  verizon.com.customer.alter.net (152.179.120.230)  14.709 ms  18.793 ms  19.101 ms
7  if-ae-12-2.tcore1.n75-new-york.as6453.net (66.110.96.5)  19.771 ms  19.279 ms  19.102 ms
8  * * *
9  * * *
10  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute undernets.me
Selected device en0, address 192.168.1.244, port 56297 for outgoing packets
Tracing the path to undernets.me (157.230.179.212) on TCP port 80 (http), 30 hops max
 1  192.168.1.1  7.641 ms  5.707 ms  3.906 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  16.461 ms  17.659 ms  21.677 ms
 4  * * *
 5  0.et-5-0-5.br2.nyc4.alter.net (140.222.1.59)  15.838 ms  13.046 ms  19.101 ms
 6  verizon.com.customer.alter.net (152.179.120.230)  19.934 ms  19.511 ms  19.059 ms
 7  if-ae-12-2.tcore1.n75-new-york.as6453.net (66.110.96.5)  20.844 ms  18.433 ms  20.250 ms
 8  * * *
 9  * * *
10  * * *
.
.
.
28  * * *
29  * * *
30  * * *
Destination not reached
```



## undernets.me via AT&T Wireless w/ PIA

### ```GRE```

```
$ traceroute -P GRE undernets.me
 1  172.20.10.1 (172.20.10.1)  5.792 ms  3.922 ms  3.687 ms
 2  172.26.96.161 (172.26.96.161)  42.936 ms * *
 3  * * *
 4  * * *
 5  * * *
 .
 .
 .
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP undernets.me
 1  172.20.10.1 (172.20.10.1)  5.193 ms  2.656 ms  1.908 ms
 2  172.26.96.161 (172.26.96.161)  112.806 ms  55.744 ms  23.783 ms
 3  172.18.112.164 (172.18.112.164)  57.341 ms  24.304 ms  35.728 ms
 4  12.249.2.33 (12.249.2.33)  40.300 ms  50.917 ms  47.242 ms
 5  12.83.172.162 (12.83.172.162)  32.217 ms  305.045 ms  37.880 ms
 6  cgr1.n54ny.ip.att.net (12.122.131.85)  35.065 ms  34.801 ms  33.426 ms
 7  192.205.34.54 (192.205.34.54)  40.298 ms  70.693 ms  47.232 ms
 8  nyk-bb3-link.telia.net (62.115.115.0)  39.490 ms * *
 9  nyk-b3-link.telia.net (62.115.140.223)  44.082 ms  33.888 ms  36.832 ms
10  * * *
11  * * *
12  * * *
13  157.230.179.212 (157.230.179.212)  28.061 ms  30.396 ms  51.808 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/undernets-me_traceroute-icmp_att-pia.png)

### ```TCP```

```
$ traceroute -P TCP undernets.me
 1  * * *
 2  * * *
 3  * * *
 .
 .
 .
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP undernets.me
1  172.20.10.1 (172.20.10.1)  5.126 ms  1.969 ms  3.022 ms
2  172.26.96.161 (172.26.96.161)  245.192 ms  33.252 ms  19.881 ms
3  172.18.112.164 (172.18.112.164)  34.636 ms  41.768 ms  43.815 ms
4  12.249.2.33 (12.249.2.33)  36.424 ms  24.536 ms  56.218 ms
5  12.83.172.162 (12.83.172.162)  57.558 ms  59.720 ms  39.679 ms
6  cgr1.n54ny.ip.att.net (12.122.131.85)  30.456 ms  50.825 ms  37.837 ms
7  192.205.34.54 (192.205.34.54)  24.892 ms  54.141 ms  40.535 ms
8  nyk-bb4-link.telia.net (80.91.254.15)  25.069 ms
   nyk-bb3-link.telia.net (62.115.115.0)  59.333 ms
   nyk-bb4-link.telia.net (80.91.254.15)  49.402 ms
9  nyk-b3-link.telia.net (62.115.139.151)  70.457 ms  42.683 ms
   nyk-b3-link.telia.net (62.115.140.223)  35.436 ms
10  * * *
11  * * *
12  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute undernets.me
Selected device en0, address 172.20.10.13, port 56078 for outgoing packets
Tracing the path to undernets.me (157.230.179.212) on TCP port 80 (http), 30 hops max
 1  172.20.10.1  297.128 ms  1.799 ms  2.710 ms
 2  172.26.96.161  44.538 ms  40.689 ms  39.735 ms
 3  157.230.179.212 [open]  40.147 ms  33.434 ms  41.780 ms
```



## undernets.me via NYU Shanghai

### ```GRE```
```
$ traceroute -P GRE undernets.me
traceroute to undernets.me (157.230.179.212), 64 hops max, 52 byte packets
 1  * * *
 2  * * *
 3  * * *
 .
 .
 .
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP undernets.me
traceroute to undernets.me (157.230.179.212), 64 hops max, 72 byte packets
 1  10.213.32.209 (10.213.32.209)  248.868 ms  306.711 ms  307.300 ms
 2  10.213.32.22 (10.213.32.22)  306.446 ms  304.439 ms  307.087 ms
 3  pc118.imagic.net (202.76.48.118)  307.690 ms  276.012 ms  289.884 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  305.670 ms  395.697 ms  320.042 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  306.969 ms  306.513 ms  307.789 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  308.377 ms  354.106 ms  280.538 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  291.647 ms  308.820 ms  306.130 ms
 8  ae-19.r02.tkokhk01.hk.bb.gin.ntt.net (129.250.5.157)  310.173 ms  307.734 ms  280.964 ms
 9  ae-5.r25.tkokhk01.hk.bb.gin.ntt.net (129.250.6.95)  285.930 ms  303.926 ms  308.620 ms
10  ae-7.r24.osakjp02.jp.bb.gin.ntt.net (129.250.2.42)  407.818 ms  320.632 ms  322.060 ms
11  ae-4.r23.sttlwa01.us.bb.gin.ntt.net (129.250.3.60)  437.413 ms  510.603 ms  416.889 ms
12  ae-0.r22.sttlwa01.us.bb.gin.ntt.net (129.250.6.29)  506.193 ms  512.131 ms  421.890 ms
13  ae-0.r24.nycmny01.us.bb.gin.ntt.net (129.250.4.14)  474.994 ms  494.368 ms  479.963 ms
14  ae-3.r00.nycmny17.us.bb.gin.ntt.net (129.250.3.49)  475.436 ms  474.185 ms  473.724 ms
15  ce-0-12-0-2.r00.nycmny17.us.ce.gin.ntt.net (157.238.179.70)  481.228 ms  481.454 ms  482.410 ms
16  * * *
17  * * *
18  157.230.179.212 (157.230.179.212)  489.834 ms  494.434 ms  531.279 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/undernets-me_traceroute-icmp_shanghai.png)

### ```TCP```

```
$ traceroute -P TCP undernets.me
traceroute to undernets.me (157.230.179.212), 64 hops max, 64 byte packets
 1  * * *
 2  * * *
 3  * * *
 .
 .
 .
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP undernets.me
traceroute to undernets.me (157.230.179.212), 64 hops max, 52 byte packets
 1  10.213.32.209 (10.213.32.209)  253.658 ms  317.029 ms  306.988 ms
 2  10.213.32.22 (10.213.32.22)  307.256 ms  306.253 ms  307.661 ms
 3  pc118.imagic.net (202.76.48.118)  307.473 ms  288.257 ms  324.532 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  280.030 ms  327.311 ms  313.210 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  308.754 ms  305.016 ms  310.347 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  304.530 ms  306.891 ms  280.539 ms
 7  * xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  344.883 ms  289.349 ms
 8  ae-19.r02.tkokhk01.hk.bb.gin.ntt.net (129.250.5.157)  324.726 ms  286.974 ms  276.350 ms
 9  ae-5.r25.tkokhk01.hk.bb.gin.ntt.net (129.250.6.95)  275.762 ms  388.566 ms  281.064 ms
10  ae-7.r24.osakjp02.jp.bb.gin.ntt.net (129.250.2.42)  333.208 ms  322.026 ms  326.008 ms
11  ae-4.r23.sttlwa01.us.bb.gin.ntt.net (129.250.3.60)  417.170 ms  448.446 ms  415.204 ms
12  ae-0.r22.sttlwa01.us.bb.gin.ntt.net (129.250.6.29)  527.510 ms  418.504 ms  502.508 ms
13  ae-0.r24.nycmny01.us.bb.gin.ntt.net (129.250.4.14)  512.016 ms  479.042 ms  544.346 ms
14  ae-3.r00.nycmny17.us.bb.gin.ntt.net (129.250.3.49)  511.958 ms  479.969 ms
    ae-1.r01.nycmny17.us.bb.gin.ntt.net (129.250.4.41)  476.361 ms
15  ce-0-13-0-2.r01.nycmny17.us.ce.gin.ntt.net (157.238.179.154)  578.014 ms
    ce-0-12-0-2.r00.nycmny17.us.ce.gin.ntt.net (157.238.179.70)  513.459 ms  510.685 ms
16  138.197.244.4 (138.197.244.4)  511.941 ms
    138.197.244.8 (138.197.244.8)  487.664 ms
    138.197.244.0 (138.197.244.0)  502.993 ms
17  * * *
18  * * *
19  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute undernets.me
Selected device utun1, address 10.208.53.148, port 56799 for outgoing packets
Tracing the path to undernets.me (157.230.179.212) on TCP port 80 (http), 30 hops max
Destination not reached
 1  10.213.32.209  330.393 ms  309.194 ms *
 2  10.213.32.22  247.478 ms  253.022 ms  285.707 ms
 3  pc118.imagic.net (202.76.48.118)  307.082 ms  313.218 ms  273.075 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  274.945 ms  344.820 ms  328.365 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  307.136 ms  306.552 ms  278.079 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  276.511 ms  365.815 ms  307.190 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  283.002 ms  331.111 ms  277.786 ms
 8  ae-19.r02.tkokhk01.hk.bb.gin.ntt.net (129.250.5.157)  336.092 ms  306.457 ms  283.144 ms
 9  ae-5.r25.tkokhk01.hk.bb.gin.ntt.net (129.250.6.95)  331.318 ms  277.828 ms  287.309 ms
10  ae-7.r24.osakjp02.jp.bb.gin.ntt.net (129.250.2.42)  355.002 ms  325.129 ms  324.949 ms
11  ae-4.r23.sttlwa01.us.bb.gin.ntt.net (129.250.3.60)  475.462 ms  511.260 ms  512.216 ms
12  ae-0.r22.sttlwa01.us.bb.gin.ntt.net (129.250.6.29)  511.764 ms  511.194 ms  511.947 ms
13  ae-0.r24.nycmny01.us.bb.gin.ntt.net (129.250.4.14)  512.178 ms  483.047 ms  473.972 ms
14  ae-1.r01.nycmny17.us.bb.gin.ntt.net (129.250.4.41)  482.257 ms  504.467 ms  511.914 ms
15  ce-0-13-0-2.r01.nycmny17.us.ce.gin.ntt.net (157.238.179.154)  515.095 ms  490.158 ms  529.606 ms
16  * * *
17  * * *
18  * * *
.
.
.
28  * * *
29  * * *
30  * * *
```



## itp.nopivnick.com via Verizon Fios

### ```GRE```

```
$ traceroute -P GRE itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.108.153
traceroute to nopivnick.github.io (185.199.108.153), 64 hops max, 52 byte packets
 1  fios_quantum_gateway (192.168.1.1)  26.959 ms  5.686 ms  5.833 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  19.976 ms  25.459 ms
    b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  25.242 ms
 4  * * *
 5  0.ae3.gw17.nyc4.alter.net (140.222.1.135)  14.885 ms  18.930 ms  19.155 ms
 6  * * *
 7  * * *
 8  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP itp.nopivnick.com
 1  fios_quantum_gateway (192.168.1.1)  4.626 ms  4.237 ms  3.957 ms
 2  185.199.111.153 (185.199.111.153)  16.724 ms  10.140 ms  9.748 ms
```

### ```TCP```

```
$ traceroute -P TCP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.108.153
traceroute to nopivnick.github.io (185.199.108.153), 64 hops max, 72 byte packets
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.108.153
traceroute to nopivnick.github.io (185.199.108.153), 64 hops max, 52 byte packets
 1  fios_quantum_gateway (192.168.1.1)  11.488 ms  6.318 ms  4.504 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  945.126 ms
    b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  20.098 ms
    b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  23.870 ms
 4  * * *
 5  0.ae3.gw17.nyc4.alter.net (140.222.1.135)  25.936 ms
    0.ae4.gw17.nyc4.alter.net (140.222.1.137)  21.318 ms  19.561 ms
 6  * * *
 7  * * *
 8  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute itp.nopivnick.com
Selected device en0, address 192.168.1.244, port 56260 for outgoing packets
Tracing the path to itp.nopivnick.com (185.199.108.153) on TCP port 80 (http), 30 hops max
 1  192.168.1.1  4.853 ms  3.965 ms  4.090 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  19.331 ms  12.694 ms  19.864 ms
 4  * * *
 5  0.ae3.gw17.nyc4.alter.net (140.222.1.135)  18.932 ms  12.917 ms  19.874 ms
 6  65.204.45.180  19.121 ms  14.889 ms  19.858 ms
 7  185.199.108.153 [open]  19.191 ms  20.221 ms  19.797 ms
```


## itp.nopivnick.com via AT&T Wireless w/ PIA

### ```GRE```

```
$ traceroute -P GRE itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.110.153
traceroute to nopivnick.github.io (185.199.110.153), 64 hops max, 52 byte packets
 1  172.20.10.1 (172.20.10.1)  5.733 ms  3.333 ms  5.045 ms
 2  * * *
 3  * * *
 4  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.110.153
traceroute to nopivnick.github.io (185.199.110.153), 64 hops max, 72 byte packets
 1  172.20.10.1 (172.20.10.1)  6.650 ms  2.061 ms  4.168 ms
 2  172.26.96.161 (172.26.96.161)  98.121 ms  50.271 ms  40.868 ms
 3  172.18.112.164 (172.18.112.164)  32.457 ms  29.509 ms  29.912 ms
 4  12.249.2.33 (12.249.2.33)  66.226 ms  34.455 ms  40.266 ms
 5  12.83.172.162 (12.83.172.162)  47.013 ms  33.361 ms  73.993 ms
 6  12.122.115.105 (12.122.115.105)  40.300 ms  32.047 ms  54.902 ms
 7  * * *
 8  185.199.110.153 (185.199.110.153)  157.576 ms  37.275 ms  34.515 ms
 1  fios_quantum_gateway (192.168.1.1)  8.250 ms  4.417 ms  4.777 ms
 2  185.199.108.153 (185.199.108.153)  19.263 ms  16.726 ms  9.713 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/itp-nopivnick-com_traceroute-icmp_att-pia.png)

### ```TCP```

```
$ traceroute -P TCP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.110.153
traceroute to nopivnick.github.io (185.199.110.153), 64 hops max, 64 byte packets
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.110.153
traceroute to nopivnick.github.io (185.199.110.153), 64 hops max, 52 byte packets
 1  172.20.10.1 (172.20.10.1)  2.664 ms  3.554 ms  3.186 ms
 2  172.26.96.161 (172.26.96.161)  24.577 ms  35.523 ms  23.673 ms
 3  172.18.112.164 (172.18.112.164)  47.998 ms  48.481 ms
    172.18.112.188 (172.18.112.188)  80.297 ms
 4  12.249.2.33 (12.249.2.33)  71.950 ms  37.661 ms  41.138 ms
 5  12.83.172.162 (12.83.172.162)  39.899 ms  36.062 ms  30.680 ms
 6  12.122.115.105 (12.122.115.105)  28.959 ms  46.143 ms  55.926 ms
 7  * * *
 8  * * *
 9  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute itp.nopivnick.com
Selected device en0, address 172.20.10.13, port 56075 for outgoing packets
Tracing the path to itp.nopivnick.com (185.199.110.153) on TCP port 80 (http), 30 hops max
 1  172.20.10.1  5.891 ms  6.391 ms  1.828 ms
 2  172.26.96.161  95.656 ms  49.247 ms  40.172 ms
 3  185.199.110.153 [open]  39.333 ms  27.867 ms  29.889 ms
```



## itp.nopivnick.com via NYU Shanghai

### ```GRE```

```
$ traceroute -P GRE itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.111.153
traceroute to nopivnick.github.io (185.199.111.153), 64 hops max, 52 byte packets
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.111.153
traceroute to nopivnick.github.io (185.199.111.153), 64 hops max, 72 byte packets
 1  10.213.32.209 (10.213.32.209)  421.012 ms  306.125 ms  248.883 ms
 2  10.213.32.22 (10.213.32.22)  263.134 ms  247.157 ms  250.670 ms
 3  pc118.imagic.net (202.76.48.118)  285.721 ms  295.153 ms  275.531 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  338.648 ms  321.154 ms  395.332 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  283.669 ms  276.511 ms  313.475 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  306.422 ms  282.717 ms  278.733 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  276.596 ms  286.640 ms  276.366 ms
 8  ae-17.r03.tkokhk01.hk.bb.gin.ntt.net (129.250.5.245)  298.246 ms  277.244 ms  334.942 ms
 9  ae-0.fastly.tkokhk01.hk.bb.gin.ntt.net (203.131.240.226)  313.065 ms  282.696 ms  307.279 ms
10  185.199.111.153 (185.199.111.153)  277.591 ms  307.698 ms  308.222 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/itp-nopivnick-com_traceroute-icmp_shanghai.png)

### ```TCP```

```
$ traceroute -P TCP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.111.153
traceroute to nopivnick.github.io (185.199.111.153), 64 hops max, 64 byte packets
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP itp.nopivnick.com
traceroute: Warning: itp.nopivnick.com has multiple addresses; using 185.199.111.153
traceroute to nopivnick.github.io (185.199.111.153), 64 hops max, 52 byte packets
 1  10.213.32.209 (10.213.32.209)  265.177 ms  254.052 ms  246.122 ms
 2  10.213.32.22 (10.213.32.22)  302.073 ms  306.201 ms  246.723 ms
 3  pc118.imagic.net (202.76.48.118)  281.239 ms  278.403 ms  275.242 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  287.992 ms  316.123 ms  282.406 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  285.017 ms  276.730 ms  335.164 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  312.840 ms  301.111 ms  306.612 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  307.302 ms  277.107 ms  337.008 ms
 8  ae-17.r03.tkokhk01.hk.bb.gin.ntt.net (129.250.5.245)  307.284 ms  281.715 ms  284.132 ms
 9  * * *
10  * * *
11  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute itp.nopivnick.com
Selected device utun1, address 10.208.53.148, port 56807 for outgoing packets
Tracing the path to itp.nopivnick.com (185.199.111.153) on TCP port 80 (http), 30 hops max
 1  10.213.32.209  291.468 ms  298.852 ms  315.428 ms
 2  10.213.32.22  246.392 ms  297.683 ms  274.691 ms
 3  pc118.imagic.net (202.76.48.118)  307.463 ms  306.032 ms  307.206 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  313.516 ms  304.580 ms  302.754 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  389.593 ms  326.370 ms  306.998 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  307.201 ms  279.283 ms  274.155 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  281.370 ms  393.854 ms  305.525 ms
 8  ae-17.r03.tkokhk01.hk.bb.gin.ntt.net (129.250.5.245)  306.860 ms  306.628 ms  307.173 ms
 9  ae-0.fastly.tkokhk01.hk.bb.gin.ntt.net (203.131.240.226)  306.961 ms  308.886 ms  371.405 ms
10  185.199.111.153 [open]  445.456 ms  295.124 ms  286.698 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/itp-nopivnick-com_tcptraceroute_shanghai.png)

## big.net via Verizon Fios

### ```GRE```

```
$ traceroute -P GRE big.net
 1  fios_quantum_gateway (192.168.1.1)  6.324 ms  3.973 ms  4.035 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  18.600 ms
    b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  21.207 ms  15.455 ms
 4  * * *
 5  0.ae9.br3.nyc4.alter.net (140.222.2.127)  24.180 ms  18.600 ms  19.285 ms
 6  verizon.com.customer.alter.net (152.179.145.234)  20.280 ms  18.470 ms  21.050 ms
 7  be3496.ccr42.jfk02.atlas.cogentco.com (154.54.0.141)  19.023 ms  27.662 ms  19.732 ms
 8  be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  38.242 ms  37.544 ms  31.526 ms
 9  be2718.ccr42.ord01.atlas.cogentco.com (154.54.7.129)  36.795 ms  29.436 ms  28.759 ms
10  be2832.ccr22.mci01.atlas.cogentco.com (154.54.44.169)  53.383 ms  55.871 ms  59.020 ms
11  be3036.ccr22.den01.atlas.cogentco.com (154.54.31.89)  70.954 ms  67.897 ms  69.211 ms
12  be3038.ccr32.slc01.atlas.cogentco.com (154.54.42.97)  83.819 ms  77.441 ms  78.246 ms
13  be3110.ccr22.sfo01.atlas.cogentco.com (154.54.44.141)  78.971 ms  77.350 ms  78.316 ms
14  be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  78.093 ms  78.541 ms  78.989 ms
15  38.104.141.82 (38.104.141.82)  78.288 ms  77.192 ms  78.350 ms
16  * * *
17  * * *
18  * * *
19  * * *
20  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  105.326 ms  78.208 ms  79.114 ms
21  * * *
22  * * *
23  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP big.net
 1  fios_quantum_gateway (192.168.1.1)  4.969 ms  5.522 ms  4.944 ms
 2  157-131-153-126.fiber.dynamic.sonic.net (157.131.153.126)  15.899 ms  16.327 ms  18.275 ms
```

### ```TCP```

```
$ traceroute -P TCP big.net
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP big.net
 1  fios_quantum_gateway (192.168.1.1)  5.365 ms  6.660 ms  4.085 ms
 2  * * *
 3  b3366.nycmny-lcr-21.verizon-gni.net (130.81.30.200)  23.592 ms  21.318 ms
    b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  19.158 ms
 4  * * *
 5  0.ae9.br3.nyc4.alter.net (140.222.2.127)  25.090 ms  13.132 ms
    0.ae10.br3.nyc4.alter.net (140.222.2.129)  18.958 ms
 6  verizon.com.customer.alter.net (152.179.145.234)  18.809 ms  21.376 ms  18.638 ms
 7  be3495.ccr41.jfk02.atlas.cogentco.com (66.28.4.181)  19.654 ms
    be3496.ccr42.jfk02.atlas.cogentco.com (154.54.0.141)  20.177 ms  18.682 ms
 8  be2889.ccr21.cle04.atlas.cogentco.com (154.54.47.49)  36.571 ms  36.199 ms
    be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  29.044 ms
 9  be2718.ccr42.ord01.atlas.cogentco.com (154.54.7.129)  34.588 ms  40.502 ms  37.236 ms
10  be2831.ccr21.mci01.atlas.cogentco.com (154.54.42.165)  50.858 ms  57.997 ms  59.350 ms
11  be3035.ccr21.den01.atlas.cogentco.com (154.54.5.89)  70.275 ms
    be3036.ccr22.den01.atlas.cogentco.com (154.54.31.89)  70.775 ms  63.245 ms
12  be3038.ccr32.slc01.atlas.cogentco.com (154.54.42.97)  82.906 ms  77.380 ms
    be3037.ccr21.slc01.atlas.cogentco.com (154.54.41.145)  79.167 ms
13  be3109.ccr21.sfo01.atlas.cogentco.com (154.54.44.137)  82.814 ms  78.379 ms  77.375 ms
14  be2015.ccr31.sjc04.atlas.cogentco.com (154.54.7.174)  79.050 ms
    be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  82.139 ms  83.137 ms
15  38.104.141.82 (38.104.141.82)  77.301 ms  82.853 ms  76.921 ms
16  102.ae1.cr1.pao1.sonic.net (70.36.205.5)  85.153 ms  99.404 ms  85.787 ms
17  0.ae0.cr1.colaca01.sonic.net (70.36.205.62)  86.650 ms  89.451 ms  91.560 ms
18  0.ae2.cr2.colaca01.sonic.net (157.131.209.66)  206.733 ms  95.210 ms  167.683 ms
19  0.ae0.cr2.snfcca05.sonic.net (198.27.244.58)  101.387 ms  107.929 ms  105.693 ms
20  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  80.727 ms  85.384 ms  230.979 ms
21  * * *
22  * * *
23  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute big.net
Selected device en0, address 192.168.1.244, port 56258 for outgoing packets
Tracing the path to big.net (157.131.153.126) on TCP port 80 (http), 30 hops max
 1  192.168.1.1  5.227 ms  4.744 ms  5.232 ms
 2  * * *
 3  b3366.nycmny-lcr-22.verizon-gni.net (130.81.30.202)  15.338 ms  12.628 ms  19.911 ms
 4  * * *
 5  0.ae10.br3.nyc4.alter.net (140.222.2.129)  17.916 ms  17.734 ms  21.097 ms
 6  verizon.com.customer.alter.net (152.179.145.234)  17.118 ms  13.149 ms  19.141 ms
 7  be3496.ccr42.jfk02.atlas.cogentco.com (154.54.0.141)  9.907 ms  12.867 ms  11.021 ms
 8  be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  36.976 ms  32.623 ms  29.036 ms
 9  be2718.ccr42.ord01.atlas.cogentco.com (154.54.7.129)  39.156 ms  37.083 ms  39.901 ms
10  be2832.ccr22.mci01.atlas.cogentco.com (154.54.44.169)  50.790 ms  59.500 ms  59.062 ms
11  be3036.ccr22.den01.atlas.cogentco.com (154.54.31.89)  70.776 ms  70.325 ms  68.254 ms
12  be3038.ccr32.slc01.atlas.cogentco.com (154.54.42.97)  83.219 ms  83.860 ms  77.188 ms
13  be3110.ccr22.sfo01.atlas.cogentco.com (154.54.44.141)  77.413 ms  75.819 ms  79.886 ms
14  be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  77.457 ms  81.401 ms  79.791 ms
15  38.104.141.82  78.291 ms  82.448 ms  78.503 ms
16  * * *
17  * * *
18  * * *
19  * * *
20  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  83.957 ms  80.763 ms  79.076 ms
21  157-131-153-126.fiber.dynamic.sonic.net (157.131.153.126) [open]  79.057 ms  82.762 ms  79.437 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/big-net_tcptraceroute_fios.png)

## big.net via AT&T Wireless w/ PIA

### ```GRE```

```
$ traceroute -P GRE big.net
 1  172.20.10.1 (172.20.10.1)  5.707 ms  3.327 ms  3.256 ms
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP big.net
 1  172.20.10.1 (172.20.10.1)  6.888 ms  2.020 ms  7.734 ms
 2  172.26.96.161 (172.26.96.161)  316.008 ms  34.049 ms  26.805 ms
 3  172.18.112.188 (172.18.112.188)  24.413 ms  47.375 ms  39.937 ms
 4  12.249.2.33 (12.249.2.33)  39.767 ms  39.576 ms  41.277 ms
 5  12.83.172.162 (12.83.172.162)  38.371 ms  40.994 ms  25.606 ms
 6  n54ny402igs.ip.att.net (12.122.131.101)  35.486 ms  38.692 ms  39.469 ms
 7  be3004.ccr31.jfk10.atlas.cogentco.com (154.54.10.97)  24.977 ms  38.013 ms  54.419 ms
 8  be3496.ccr42.jfk02.atlas.cogentco.com (154.54.0.141)  25.865 ms  22.100 ms  30.672 ms
 9  be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  66.261 ms  41.485 ms  56.853 ms
10  be2718.ccr42.ord01.atlas.cogentco.com (154.54.7.129)  59.578 ms  39.733 ms  77.138 ms
11  be2832.ccr22.mci01.atlas.cogentco.com (154.54.44.169)  80.256 ms  79.538 ms  79.605 ms
12  be3036.ccr22.den01.atlas.cogentco.com (154.54.31.89)  84.153 ms  84.868 ms  193.792 ms
13  be3038.ccr32.slc01.atlas.cogentco.com (154.54.42.97)  103.944 ms  100.805 ms  101.290 ms
14  be3110.ccr22.sfo01.atlas.cogentco.com (154.54.44.141)  98.542 ms  98.304 ms  257.373 ms
15  be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  102.777 ms  198.621 ms  110.129 ms
16  38.104.141.82 (38.104.141.82)  119.180 ms  308.239 ms  305.754 ms
17  102.ae1.cr1.pao1.sonic.net (70.36.205.5)  306.635 ms  213.010 ms  401.735 ms
18  0.ae0.cr1.colaca01.sonic.net (70.36.205.62)  156.336 ms  217.769 ms  310.084 ms
19  0.ae2.cr2.colaca01.sonic.net (157.131.209.66)  391.915 ms  198.662 ms  414.738 ms
20  0.ae0.cr2.snfcca05.sonic.net (198.27.244.58)  306.638 ms  180.673 ms  427.404 ms
21  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  173.710 ms  146.241 ms  328.583 ms
22  157-131-153-126.fiber.dynamic.sonic.net (157.131.153.126)  306.930 ms  308.634 ms  306.042 ms
```

Here's a map of the trace:

![image-title-here](/assets/images/undernets/week-04/big-net_traceroute-icmp_att-pia.png)

### ```TCP```

```
$ traceroute -P TCP big.net
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP big.net
 1  172.20.10.1 (172.20.10.1)  6.141 ms  3.195 ms  1.913 ms
 2  172.26.96.161 (172.26.96.161)  57.746 ms  46.229 ms  41.661 ms
 3  172.18.112.164 (172.18.112.164)  38.281 ms
    172.18.112.188 (172.18.112.188)  33.401 ms  47.386 ms
 4  12.249.2.33 (12.249.2.33)  39.860 ms  34.603 ms  39.524 ms
 5  12.83.172.162 (12.83.172.162)  34.697 ms  44.114 ms  40.278 ms
 6  n54ny402igs.ip.att.net (12.122.131.101)  29.918 ms  38.165 ms  40.452 ms
 7  be3004.ccr31.jfk10.atlas.cogentco.com (154.54.10.97)  24.541 ms  37.978 ms  54.406 ms
 8  be3496.ccr42.jfk02.atlas.cogentco.com (154.54.0.141)  25.858 ms  21.972 ms  30.791 ms
 9  be2889.ccr21.cle04.atlas.cogentco.com (154.54.47.49)  66.210 ms
    be2890.ccr22.cle04.atlas.cogentco.com (154.54.82.245)  48.220 ms
    be2889.ccr21.cle04.atlas.cogentco.com (154.54.47.49)  50.840 ms
10  be2718.ccr42.ord01.atlas.cogentco.com (154.54.7.129)  58.654 ms  40.006 ms
    be2717.ccr41.ord01.atlas.cogentco.com (154.54.6.221)  76.191 ms
11  be2832.ccr22.mci01.atlas.cogentco.com (154.54.44.169)  79.395 ms
    be2831.ccr21.mci01.atlas.cogentco.com (154.54.42.165)  78.961 ms
    be2832.ccr22.mci01.atlas.cogentco.com (154.54.44.169)  76.904 ms
12  be3035.ccr21.den01.atlas.cogentco.com (154.54.5.89)  194.248 ms  81.502 ms
    be3036.ccr22.den01.atlas.cogentco.com (154.54.31.89)  81.664 ms
13  be3038.ccr32.slc01.atlas.cogentco.com (154.54.42.97)  118.899 ms  117.357 ms  213.455 ms
14  be3109.ccr21.sfo01.atlas.cogentco.com (154.54.44.137)  112.139 ms
    be3110.ccr22.sfo01.atlas.cogentco.com (154.54.44.141)  97.661 ms
    be3109.ccr21.sfo01.atlas.cogentco.com (154.54.44.137)  256.639 ms
15  be2015.ccr31.sjc04.atlas.cogentco.com (154.54.7.174)  102.158 ms
    be2016.ccr31.sjc04.atlas.cogentco.com (154.54.0.178)  198.606 ms  109.403 ms
16  38.104.141.82 (38.104.141.82)  119.202 ms  308.325 ms  305.713 ms
17  102.ae1.cr1.pao1.sonic.net (70.36.205.5)  306.654 ms  212.963 ms  401.881 ms
18  0.ae0.cr1.colaca01.sonic.net (70.36.205.62)  156.252 ms  217.772 ms  310.101 ms
19  0.ae2.cr2.colaca01.sonic.net (157.131.209.66)  391.946 ms  201.471 ms  411.718 ms
20  0.ae0.cr2.snfcca05.sonic.net (198.27.244.58)  306.647 ms  181.306 ms  427.368 ms
21  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  173.714 ms  146.783 ms  328.056 ms
22  * * *
23  * * *
24  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute big.net
Selected device en0, address 172.20.10.13, port 56082 for outgoing packets
Tracing the path to big.net (157.131.153.126) on TCP port 80 (http), 30 hops max
 1  172.20.10.1  4.067 ms  1.842 ms  2.762 ms
 2  172.26.96.161  27.878 ms  58.724 ms  37.807 ms
 3  157-131-153-126.fiber.dynamic.sonic.net (157.131.153.126) [open]  40.991 ms  38.297 ms  26.576 ms
```



## big.net via NYU Shanghai

### ```GRE```

```
$ traceroute -P GRE big.net
 traceroute to big.net (157.131.153.126), 64 hops max, 52 byte packets
 1  * * *
 2  * * *
 3  * * *
 .
 .
 .
62  * * *
63  * * *
64  * * *
```

### ```ICMP```

```
$ traceroute -P ICMP big.net
traceroute to big.net (157.131.153.126), 64 hops max, 72 byte packets
 1  10.213.32.209 (10.213.32.209)  248.459 ms  246.968 ms  245.615 ms
 2  10.213.32.22 (10.213.32.22)  245.820 ms  246.372 ms  297.886 ms
 3  pc118.imagic.net (202.76.48.118)  281.090 ms  291.034 ms  277.997 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  276.539 ms  276.232 ms  338.046 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  305.768 ms  288.372 ms  281.925 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  280.596 ms  274.630 ms  308.238 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  408.800 ms  585.916 ms  283.239 ms
 8  ae-17.r03.tkokhk01.hk.bb.gin.ntt.net (129.250.5.245)  359.959 ms  281.109 ms  285.434 ms
 9  ae-11.r24.tkokhk01.hk.bb.gin.ntt.net (129.250.6.99)  354.635 ms  336.256 ms  307.927 ms
10  ae-12.r30.tokyjp05.jp.bb.gin.ntt.net (129.250.2.50)  398.903 ms  411.639 ms  409.607 ms
11  ae-4.r23.snjsca04.us.bb.gin.ntt.net (129.250.5.78)  493.698 ms  463.562 ms  431.094 ms
12  ae-41.r02.snjsca04.us.bb.gin.ntt.net (129.250.6.119)  437.824 ms  435.341 ms  434.176 ms
13  ae-0.cogent.snjsca04.us.bb.gin.ntt.net (129.250.8.42)  434.798 ms  476.877 ms  434.509 ms
14  be2013.ccr31.sjc04.atlas.cogentco.com (154.54.5.106)  431.800 ms  441.391 ms  436.800 ms
15  38.104.141.82 (38.104.141.82)  432.002 ms  508.037 ms  518.445 ms
16  102.ae1.cr1.pao1.sonic.net (70.36.205.5)  432.474 ms  439.068 ms  433.439 ms
17  0.ae0.cr1.colaca01.sonic.net (70.36.205.62)  561.833 ms  580.355 ms  545.883 ms
18  0.ae2.cr2.colaca01.sonic.net (157.131.209.66)  463.226 ms  454.820 ms  450.513 ms
19  0.ae0.cr2.snfcca05.sonic.net (198.27.244.58)  464.770 ms  461.416 ms  489.913 ms
20  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  441.640 ms  486.208 ms  437.133 ms
21  * * *
22  * * *
23  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```TCP```

```
$ traceroute -P TCP big.net
traceroute to big.net (157.131.153.126), 64 hops max, 64 byte packets
 1  * * *
 2  * * *
 3  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```UDP```

```
$ traceroute -P UDP big.net
traceroute to big.net (157.131.153.126), 64 hops max, 52 byte packets
 1  10.213.32.209 (10.213.32.209)  324.451 ms  307.213 ms  305.903 ms
 2  10.213.32.22 (10.213.32.22)  249.682 ms  364.389 ms  305.759 ms
 3  pc118.imagic.net (202.76.48.118)  288.699 ms  276.481 ms  278.646 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  286.316 ms  304.455 ms  407.655 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  586.552 ms  282.170 ms  360.280 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  280.672 ms  275.500 ms  364.759 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  285.388 ms *  320.001 ms
 8  ae-19.r02.tkokhk01.hk.bb.gin.ntt.net (129.250.5.157)  277.057 ms  357.045 ms
    ae-17.r03.tkokhk01.hk.bb.gin.ntt.net (129.250.5.245)  289.990 ms
 9  ae-10.r24.tkokhk01.hk.bb.gin.ntt.net (129.250.6.93)  287.694 ms
    ae-11.r24.tkokhk01.hk.bb.gin.ntt.net (129.250.6.99)  386.483 ms
    ae-10.r24.tkokhk01.hk.bb.gin.ntt.net (129.250.6.93)  281.560 ms
10  ae-12.r30.tokyjp05.jp.bb.gin.ntt.net (129.250.2.50)  321.963 ms  324.808 ms  322.577 ms
11  ae-4.r23.snjsca04.us.bb.gin.ntt.net (129.250.5.78)  447.151 ms  447.701 ms  431.542 ms
12  ae-41.r02.snjsca04.us.bb.gin.ntt.net (129.250.6.119)  492.933 ms  434.904 ms  433.920 ms
13  ae-0.cogent.snjsca04.us.bb.gin.ntt.net (129.250.8.42)  434.782 ms  433.854 ms  439.200 ms
14  be2013.ccr31.sjc04.atlas.cogentco.com (154.54.5.106)  486.474 ms  431.741 ms  474.439 ms
15  38.104.141.82 (38.104.141.82)  435.826 ms  447.801 ms  434.100 ms
16  102.ae1.cr1.pao1.sonic.net (70.36.205.5)  537.545 ms  509.038 ms  516.691 ms
17  0.ae0.cr1.colaca01.sonic.net (70.36.205.62)  609.073 ms  448.686 ms  457.320 ms
18  0.ae2.cr2.colaca01.sonic.net (157.131.209.66)  459.207 ms  454.688 ms  450.456 ms
19  0.ae0.cr2.snfcca05.sonic.net (198.27.244.58)  466.116 ms  450.724 ms  500.545 ms
20  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  436.107 ms  485.978 ms  432.397 ms
21  * * *
22  * * *
23  * * *
.
.
.
62  * * *
63  * * *
64  * * *
```

### ```tcptraceroute```

```
$ sudo tcptraceroute big.net
Selected device utun1, address 10.208.53.148, port 56816 for outgoing packets
Tracing the path to big.net (157.131.153.126) on TCP port 80 (http), 30 hops max
Destination not reached
 1  10.213.32.209  247.840 ms  289.172 ms  308.042 ms
 2  10.213.32.22  249.830 ms  261.056 ms  307.275 ms
 3  pc118.imagic.net (202.76.48.118)  276.522 ms  337.799 ms  281.218 ms
 4  ip202-76-18-120.ip.hk.net (202.76.18.120)  275.846 ms  362.712 ms  307.287 ms
 5  ip202-76-18-10.ip.hk.net (202.76.18.10)  307.035 ms  282.799 ms  275.110 ms
 6  ip202-76-18-66.ip.hk.net (202.76.18.66)  291.891 ms  284.532 ms  280.199 ms
 7  xe-0-0-6-2-201.a01.newthk02.hk.bb.gin.ntt.net (203.131.241.129)  276.477 ms  284.417 ms  284.584 ms
 8  ae-19.r02.tkokhk01.hk.bb.gin.ntt.net (129.250.5.157)  287.630 ms  323.316 ms  299.732 ms
 9  ae-10.r24.tkokhk01.hk.bb.gin.ntt.net (129.250.6.93)  281.755 ms  297.779 ms  281.400 ms
10  ae-12.r30.tokyjp05.jp.bb.gin.ntt.net (129.250.2.50)  370.646 ms  323.200 ms  323.585 ms
11  ae-4.r23.snjsca04.us.bb.gin.ntt.net (129.250.5.78)  432.425 ms  554.100 ms  427.536 ms
12  ae-41.r02.snjsca04.us.bb.gin.ntt.net (129.250.6.119)  445.767 ms  456.973 ms  443.785 ms
13  ae-0.cogent.snjsca04.us.bb.gin.ntt.net (129.250.8.42)  449.603 ms  432.985 ms  431.273 ms
14  be2013.ccr31.sjc04.atlas.cogentco.com (154.54.5.106)  494.240 ms  429.608 ms  475.619 ms
15  38.104.141.82  527.748 ms  435.256 ms  484.515 ms
16  * * *
17  * * *
18  * * *
19  * * *
20  301.ae0.bras1.snfcca05.sonic.net (198.27.186.212)  453.819 ms  511.449 ms  433.271 ms
21  * * *
22  * * *
23  * * *
.
.
.
28  * * *
29  * * *
30  * * *
```
