---
title: "Setup a Client to Site VPN With OpenVPN"
date: 2022-11-12T16:09:11+05:30
draft: false
---

# Hi There :wave:

So this is one of my first articles since I've started writing. Today we'll see how to setup a Client To Site VPN connectivity Between your local system to your Cloud Provider.

The other day I was setting up a new Google Cloud VM instance for a code deployment requirement, and suddenly got a Security Breach Alert from my company's Cyber Security operations centre that there's a SSH brute force attack going on on one of our VM instances. 
:scream: :scream: :scream:

I was like....

<!-- ![Whaaaat](https://media.tenor.com/JWHXlA4ug-IAAAAC/stewie-griffin-say-what.gif) -->

<p align="center">
  <img src="https://media.tenor.com/JWHXlA4ug-IAAAAC/stewie-griffin-say-what.gif" />
</p>


My Bad.. I had allowed SSH traffic from 0.0.0.0/0 to the instance for logging into the server. :sweat:

So this brings down the question... How do we securely manage our cloud infrastrucure ???

Well back in the old days SysAdmins used to connect securely with their on-prem datacentres even via the open-Internet.

I'm not quoting Integrated Lights Out (ILO) here.. that is one of the ways, But we used VPN to connect with our datacentres present in different locations.

Hmm.. How bout' we do the same thing with our Cloud Providers :raised_hands:

There are a couple of concepts Like 
1. Establishing a Site to Site VPN
2. Establishing a Client to Site VPN
etc. etc. etc. 

Well Majorly they use IPSec protocol for Encrytion of traffic to and from our infrastructure.. 

## _Site to Site VPN_

![STS VPN](https://docs.aws.amazon.com/images/vpn/latest/s2svpn/images/vpn-basic-diagram.png)

Now here we've got Two sites 
1. On-Prem Datacentres
2. AWS Datacenteres.

Setting up a VPN tunnel between them is pretty easy.. You just spin up some customer gateways on both sides,
Create an IPSec Tunnel and make sure their MTU's are the same.. You've got yourself a VPN Tunnel.
Users can connect to the On-prem VPN server via a client on their machine and access cloud resources like VMs/Databases etc. from their private IP like its hosted in your local network..

**Definetly Giving an Edge to the Developers.. IAM roles go out the Window** :laughing: 

But the downside to this approach is that you need to maintain an on-premise server 24/7 - 365 days to ensure your VPN stays up and your customers aren't affected.

you need server support personel, maintain the physical server daily, take care of cooling, electricity , Not to mention the commitment that comes with it. It's like a goddamn marraige :cold_sweat:


__HERE COMES THE BADASS!!!__

## _Client to Site VPN_
Spoiler Alert !! It comes with very less or no long term commitments..you can break-up anytime you want :broken_heart:

Okay enough talk.. So what makes client to site VPN so cool ?

Basically The VPN server which was hosted on your on-prem is now moved to Cloud..

![CTS GW](https://d2908q01vomqb2.cloudfront.net/5b384ce32d8cdef02bc3a139d4cac0a22bb029e8/2020/04/14/Screen-Shot-2020-04-07-at-10.30.06-AM.png)


The server acts as a VPN Gateway hosted in your VPC's Subnet. Users can download the respective VPN client and authorize themselves with the server and _Voila !!!_  They can now access their favorite Cloud resources using  Private IP address...

With OpenVPN you can do just the same and be UP and running in a few Minutes..

Here are a couple of steps to get started with OpenVPN in GCP..

1. Deploy a OpenVPN Access Server in the VPC you want to setup a VPN connection with by going to GCP Marketplace and searching for OpenVPN.

2. SSH into the server and change Temporary password by `sudo passwd openvpn`

3. Login to OpenVPN admin by `http://your instance ip/admin`

4. Create a User By going into user management

5. Go to `http://your instance ip/` and Login with user credentials created above and download your compatible VPN client.

6. Also Download your `.ovpn` access file

7. Use the access file to connect to your OpenVPN Server.

8. Enjoy Doing SSH to your EC2 instance / Mysql to your Database using their Private endpoints.

![openvpn](/aroop-gochhayat/openvpn.png)

## Conclusion
Well, I love the way it securely provides access to our cloud infrastructure and definetly gives an edge to the developers devloping and connecting servers/databases while development for a real time Integration testing and all.. Feels good..

Thank You :v:



