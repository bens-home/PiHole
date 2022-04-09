# PiHole

This is a PiHole DNS server in a docker container that I can use on my home network. It uses the [pihole/pihole](https://hub.docker.com/r/pihole/pihole) docker image.


# Secrets and Login to web interface

The password to the pihole web interface is stored with a docker secret, so you will have to set that up. 

To set up your machine for a secret use the commands: 
```
docker swarm init
```
This will make your machine a docker swarm manager, which allows it to store secrets.

Now you can set the Docker secret password with :

```
echo "SuperSecurePassword" | docker secret create pihole_password -
```

Replacing the `SuperSecurePassword` with your password.

You can use `docker secret ls` to list all the secrets on your machine. Neat!