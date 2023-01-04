# Fullstackattack

#!/bin/bash
# Get the IP addresses of all interfaces
IP_ADDRS=$(ip addr | grep "inet " | cut -d" " -f6 | cut -d"/" -f1)
# Extract the network address for each IP address
NETWORKS=()
for IP_ADDR in $IP_ADDRS; do
  # Skip the loopback interface
  if [ "$IP_ADDR" == "127.0.0.1/8" ]; then
    continue
  fi
  NETWORK=$(echo $IP_ADDR | cut -d"." -f1-3).0
  NETWORKS+=($NETWORK)
done
# Perform a ping scan of each network
for NETWORK in $NETWORKS; do
  nmap -sn -n $NETWORK >> network-devices.txt
done
