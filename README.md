# PiHole

This is a PiHole DNS server in a docker container that I can use on my home network. It uses the [pihole/pihole](https://hub.docker.com/r/pihole/pihole) docker image.

# Getting Started

# Secrets and Login to web interface

The compose file is set up to use the contents of `pihole_password.txt` as the PiHole web interface password as a Docker secret.
Just make a file called `pihole_password.txt` in `docker` folder and you are good to go!

This file is ignored by the `.gitignore`, so no worries there.

## Reset Password

If you have forgotten your password or otherwise need to reset it, you can do it like this: 

```
sudo docker exec -it pihole bash
pihole -a -p
```

Which will run bash on the PiHole container and run the password reset command.

# Host Machine network settings

We need port `53` open for PiHole to properly handle DNS redirects. This also uses port `80`, so
if you have any other existing web services running there then you will need to either change that
port in the `docker/docker-compose.yml` file or move your services :)

On Ubuntu:

```
# Stop dns to free up port 53
sudo systemctl stop systemd-resolved.service

# Disable dns on start up so that pihole can use port 53 instead
sudo systemctl disable systemd-resolved.service 
```

This will stop your DNS services and free up port 53. You can check that there is nothing else running on port 53 with this command:
```
sudo lsof -i :53
```

Once this is done then you can just start the PiHole container! 
```
docker-compose up -d
```

# Firewall settings

To use the web interface from other computers you need to allow port 80 through the firewall

These rules come from the official [PiHole documentation](https://docs.pi-hole.net/main/prerequisites/)
```
sudo ufw allow 80/tcp comment "For PiHole web interface"
sudo ufw allow 53/tcp comment "For PiHole DNS"
sudo ufw allow 53/udp comment "For PiHole DNS"
sudo ufw allow 67/tcp comment "For PiHole DNS"
sudo ufw allow 67/udp comment "For PiHole DNS"

sudo ufw reload
```

# Web Interface

Go to `http://Ip.Of.Host.Machine/admin/index.php` to get to your admin page to manage your PiHole! Woot

# Tell the clients to use this as DNS instead of the default

Now to make the devices on your network use this as the dns, set it as your DNS server on your router!


# Updating PiHole

Updating PiHole can be weird. You might need to enable `systemd-resolved.service` on your host machine after you shut down the PiHole docker image to make sure that you can actually resolve the address to the docker images. 