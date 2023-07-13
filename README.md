# thearrstack
# Prep Work:

1. Create the following directory structure on the Host server:
/mnt/user/data
/mnt/user/data/dowloads
/mnt/user/data/torrents
/mnt/user/media
/mnt/user/media/movies
/mnt/user/media/series
/mnt/user/appdata/binhex-qbittorrentvpn
/mnt/user/appdata/binhex-qbittorrentvpn/openvpn

2. Obtain service credentials from your VPN provider
   
3. Download the ovpn file from your VPN Provider and copy to the following path:
/mnt/user/adddata/binhex-qbittorrentvpn/openvpn

4. Create a IPVLAN Network on Docker. This is not createad as part of the docker-compose.yaml as this network is common to the entire docker environment and should match your local network addressing.

In this example:

[IP ADDRESS] = IP Address to be used by the container. This IP should be within your local network address space

[VPN Service USERNAME] = VPN Service account name. Note: If you are using NordVPN, this is NOT your username.

[VPN Service Password] = VPN Service Password

ipvlan1: This should match the IPVLAN name created on stem 4
