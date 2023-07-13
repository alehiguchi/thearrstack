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

docker-compose.yml
version: '3'
services:
  binhex-qbittorrentvpn:
    container_name: binhex-qbittorrentvpn
    networks:
      ipvlan1:
        ipv4_address: [IP ADDRESS]
    privileged: true
    restart: unless-stopped
    environment:
      - TZ=America/Los_Angeles
      - HOST_CONTAINERNAME=binhex-qbittorrentvpn
      - TCP_PORT_6881=6881
      - UDP_PORT_6881=6881
      - TCP_PORT_8080=8080
      - TCP_PORT_8118=8118
      - VPN_ENABLED=yes
      - VPN_USER=[VPN Service USERNAME]
      - VPN_PASS=[VPN Service Password]
      - VPN_PROV=custom
      - VPN_CLIENT=openvpn
      - VPN_OPTIONS=
      - STRICT_PORT_FORWARD=yes
      - ENABLE_PRIVOXY=no
      - WEBUI_PORT=8080
      - LAN_NETWORK=192.168.1.0/24
      - NAME_SERVERS=84.200.69.80,37.235.1.174,1.1.1.1,37.235.1.177,84.200.70.40,1.0.0.1
      - VPN_INPUT_PORTS=
      - VPN_OUTPUT_PORTS=
      - DEBUG=false
      - UMASK=000
      - PUID=99
      - PGID=100
    volumes:
      - /mnt/user/data/torrents:/downloads:rw
      - /mnt/user/appdata/binhex-qbittorrentvpn:/config:rw
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
    image: binhex/arch-qbittorrentvpn

networks:
  ipvlan1:
    external: true
