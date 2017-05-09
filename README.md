# Networking Training

## Fundamentals

One way to visualize the Internet is to think about a physical map brought to life - people are inside rooms in buildings, and small roads connect these buildings. Zooming out, we see neighborhoods, and cities, and larger roads connecting these cities. Zooming back in, we see security guards who protect traffic into and out of some of the buildings, and other buildings that anyone can go into and out of. We see people using a common language to communicate with each other, and we see different people with different jobs zipping around to do different things.

Before the invention of GPS, how would a person have moved from point A to point B on this map? Let's imagine this with a specific scenario:

    Evan is at work at Galvanize in Denver, CO, and needs to drive to Blue Rocket in San Francisco, CA to deliver a training on Internet Networking.

How would Evan do this, before he had an iPhone that told him what to do?

1. Evan doesn't know the address of Galvanize by heart. Fortunately, he has a phone book from San Francisco - he flips through the phone book and finds out that Galvanize is located at 233 Sansome St Suite 1100, San Francisco, CA
2. Evan parked his car in the parking garage downstairs, so he walks down to his car and drives to the exit of the garage
3. At the garage exit, he asks the attendant about the best way to get to San Francisco. The parking attendant says "Well, I'm not an all-seeing oracle, but I know that you need to get on I-70 and west go from there. The best way to get there from here is to go South on Federal, take 6 West, and merge onto I-70. I know you'll need to go at least as far as Green River, Utah."
4. Evan follows these helpful instructions, and drives west through the Rocky mountains, arriving in Green River that evening. There, he pulls in to a gas station to refill his car, and asks the cashier about the best way to get to San Francisco. The cashier tells him that he'd normally recommend taking Route 50 to Reno, but he knows there happens to be construction going on, so the best way is probably to take 191 up to Salt Lake City, and then catch I-80 over to Reno.
5. Evan gets to Reno, and another helpful gas station attendant tells him that he can safely follow I-80 all the way to San Francisco from there.
6. Evan drives over the Bay Bridge into SF and manages to find the Philz Coffee shop on Market. There, the barista tells him he can get to 233 Sansome St by following Market St to Pine St, and then taking a right on Sansome.
7. At 233 Sansome, Evan is greeted by a friendly security guard. Access to the building is protected, but fortunately Evan has printed out a letter from David that gives him secure access to Suite 1100, so the guard waives him through
8. Evan rides the elevator up to the 11th floor, and arrives at his destination!

Woo hoo!! 🙌

Looking at this example, there are a number of metaphors that can help us understand how Internet networking works:

* A **DNS Server** is kind of like a phone book. It maps a **domain name** (google.com) to an actual **IP address** (172.217.5.110) on a physical **host**.
* A **private network** is kind of like the inside of a building, allowing a specific set of IP addresses to be reused. In our real world example, there could be a lot of parking garages, but Evan only needs to know how to get to the parking garage in his building.
* In a **public network** on the other hand, every IP address needs to be unique, just as Evan needed to know that he'd get to Blue Rocket if he travelled to 233 Sansome St in San Francisco.
* A **gateway** is the interface between two networks (often times, between a private network and a public network) - if you need to go somewhere outside of your network, you go to the gateway and get directions from there.
* A **router** joins different networks together, and is capable of routing traffic the most efficient way (just as the helpful gas station attendant diverted Evan up to Salt Lake City).
* A **firewall** can be set up to protect a private network, allowing only certain authorized traffic through (just as the security guard could have prevented Evan from travelling to the 11th floor)
* A **port** can be set up as an interface on a host machine to allow certain types of communication - just as Evan could only give his training for Blue Rocket on the 11th floor of 233 Sansome St.

## Examples

Let's look at these networking fundamentals with some specific examples.

If you're on a Mac, you can inspect your network configuration with the `ifconfig` command:

    $ ifconfig
    ...
    ether 2c:f0:ee:1d:66:be
    inet6 fe80::141d:5a0f:fb91:2498%en0 prefixlen 64 secured scopeid 0x4
    inet 10.6.16.139 netmask 0xfffff000 broadcast 10.6.31.255
    nd6 options=201<PERFORMNUD,DAD>
    media: autoselect
    status: active
    ...

There are a few important pieces of information here. First, I can see that my *IP address* is *10.6.16.139*. From experience, I know that IP addresses starting with _10_ are [reserved for private networks](https://en.wikipedia.org/wiki/Private_network), so I know that I am on a private, not a public, network:

**Private IPv4 address space:**
<table>
    <tr>
        <td>IP Address Range</td>
        <td>Number of addresses</td>
        <td>Largest CIDR block</td>
    </tr>
    <tr>
      <td>10.0.0.0 – 10.255.255.255</td>
      <td>16,777,216</td>
      <td>10.0.0.0/8 (255.0.0.0)</td>
    </tr>
    <tr>
      <td>172.16.0.0 – 172.31.255.255</td>
      <td>1,048,576</td>
      <td>172.16.0.0/12 (255.240.0.0)</td>
    </tr>
    <tr>
      <td>192.168.0.0 – 192.168.255.255</td>
      <td>65,536</td>
      <td>192.168.0.0/16 (255.255.0.0)</td>
    </tr>
</table>

This table also helps us understand some important concepts in Internet networks, so let's dive in a bit deeper.

### IPv4 addresses

IPv4 addresses are denoted by four sets of three digits, such as my IP address above:

    10.6.16.139

These addresses _actually_ represent a 32-bit number, with each of the four segments representing an 8-bit block. There are a little over 4 billion possible combinations:

    2 ^ 32 = 4,294,967,296

That's a lot of possible IP addresses, but it's easy to see how we could quickly run out with all of the devices that are on the Internet these days. One (of many) reason we have private networks is to allow some of these IP addresses to be recycled. So *anyone* in the world could create a private network in the range *10.0.0.0 - 10.255.255.255*, but only one computer on that network could have my IP address

Even with that, you can see we'd run out of IP addresses pretty quickly. A newer standard called **IPv6** has been set up with a much larger range of addresses, but most networks still support IPv4 and it's a bit easier to understand so we'll stick with that for now.

### Subnet masks

Subnet masks help define the size of a network. The larger the mask, the more addresses are _not_ part of the network. Subnet masks can be denoted in terms of _bits_ (eg, 12), a _netmask_ (eg, 255.240.0.0), or a _CIDR_ mask:

<table>
    <tr>
        <td>Mask size (bits)</td>
        <td>Netmask</td>
        <td>CIDR</td>
        <td>Number of addresses</td>
    <tr>
    <tr>
        <td>31</td>
        <td>255.255.255.252</td>
        <td>FFFFFFFF</td>
        <td>4</td>
    <tr>
    <tr>
        <td>24</td>
        <td>255.255.255.0</td>
        <td>FFFFFF00</td>
        <td>256</td>
    <tr>
    <tr>
        <td>20</td>
        <td>255.255.240.0</td>
        <td>FFFFF000</td>
        <td>4,096</td>
    <tr>
</table>

Within a given subnet mask, any computer on the network defined by that subnet can reach every other computer on that network directly, without needing to travel across the network boundary. This is one reason that you're more vulnerable to a hacker sitting at the same coffee shop as you, than one who is sitting in another part of the world on a different network.

Looking at my output from `ifconfig` again:

    $ ifconfig
    ...
    ether 2c:f0:ee:1d:66:be
    inet6 fe80::141d:5a0f:fb91:2498%en0 prefixlen 64 secured scopeid 0x4
    inet 10.6.16.139 netmask 0xfffff000 broadcast 10.6.31.255
    nd6 options=201<PERFORMNUD,DAD>
    media: autoselect
    status: active
    ...

... I can see that my network has a subnet mask of `fffff000`, which means it could host a total of 4,096 devices.

### Gateway

Gateways play a special role within networks, and outside of them (remember our parking lot attendant who helped Evan find out where to go after leaving Galvanize).

On a Mac, you can look up your gateway with the `route default` command:

    $ route get default
    route to: default
    destination: default
    mask: default
    gateway: 10.6.16.1
    interface: en0

You can test your connection to the gateway with the `ping` command (`ping` is a really useful utility for checking the connection to any DNS address or IP address):

    $ ping 10.6.16.1
    PING 10.6.16.1 (10.6.16.1): 56 data bytes
    64 bytes from 10.6.16.1: icmp_seq=0 ttl=64 time=18.024 ms
    64 bytes from 10.6.16.1: icmp_seq=1 ttl=64 time=4.295 ms
    64 bytes from 10.6.16.1: icmp_seq=2 ttl=64 time=2.978 ms
    64 bytes from 10.6.16.1: icmp_seq=3 ttl=64 time=1.812 ms
    64 bytes from 10.6.16.1: icmp_seq=4 ttl=64 time=2.952 ms
    ^C
    --- 10.6.16.1 ping statistics ---
    5 packets transmitted, 5 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 1.812/6.012/18.024/6.057 ms

I can see that my computer has a stable, fast connection to the gateway (the fact that the time is only ~4ms is pretty fast).

On any given network, the gateway is usually the _router_, and will help direct traffic between devices (so, if I want to print to a printer on _10.6.2.90_, this traffic will go to the router, and then get directed straight to the printer). Sometimes, virtual networks will be set up _inside_ a given computer - in that case the computer's network interface will act as the gateway. In my case, I have a private network set up that allows _Virtualbox_ to run virtual machines:

    $ ifconfig
    ...
    vboxnet0: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
	  ether 0a:00:27:00:00:00
	  inet 192.168.99.1 netmask 0xffffff00 broadcast 192.168.99.255

We'll come back to that in a bit!

### Domain Names

Before our eyes glaze over any further with all of these permutations of powers of 2, what does this have to do with the Internet browsing I'm familiar with?




* basics of networking (ip addresses, subnets, gateways, domain names, dns servers)
* tcp/ip protocol
* HTTP(S) protocol
* REST requests
* SSH and SCP
