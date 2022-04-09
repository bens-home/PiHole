# PiHole

This is a PiHole DNS server in a docker container that I can use on my home network. It uses the [pihole/pihole](https://hub.docker.com/r/pihole/pihole) docker image.

# Getting Started

# #Secrets and Login to web interface

The password to the PiHole web interface is stored with a docker secret, so you will have to set that up. 

Create a file called `pihole_password.txt` in the `docker` directory with the password you want inside of it.

Even when running in swarm mode Docker will store any secrets as plain text in a file somewhere, so this is assuming
that your machine is secure and nobody can see this file.

This file is ignored by the git ignore, so no worries there!

# UFW and firewall settings



# Tell the clients to use this as DNS instead of the default

Set the router's default DNS server to be the address of the machine running PiHole