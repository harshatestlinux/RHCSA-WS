# Networking

## Network Configuration
- `ip addr show` - Show IP addresses
- `ip addr add 192.168.1.100/24 dev eth0` - Add IP address
- `ip addr del 192.168.1.100/24 dev eth0` - Remove IP address
- `ip route show` - Show routing table
- `ip route add default via 192.168.1.1` - Add default route
- `ip link show` - Show network interfaces
- `ip link set eth0 up/down` - Enable/disable interface

## NetworkManager
- `nmcli connection show` - Show all connections
- `nmcli connection show --active` - Show active connections
- `nmcli device status` - Show device status
- `nmcli connection add type ethernet con-name mycon ifname eth0` - Add connection
- `nmcli connection modify mycon ipv4.addresses 192.168.1.100/24` - Set IP
- `nmcli connection modify mycon ipv4.gateway 192.168.1.1` - Set gateway
- `nmcli connection modify mycon ipv4.dns 8.8.8.8` - Set DNS
- `nmcli connection modify mycon ipv4.method manual` - Set manual IP
- `nmcli connection up mycon` - Activate connection
- `nmcli connection down mycon` - Deactivate connection

## Network Files
- `/etc/sysconfig/network-scripts/ifcfg-*` - Interface configuration files
- `/etc/resolv.conf` - DNS resolver configuration
- `/etc/hosts` - Static hostname to IP mappings
- `/etc/hostname` - System hostname
- `/etc/nsswitch.conf` - Name service switch configuration

## Hostname Management
- `hostnamectl` - Show hostname information
- `hostnamectl set-hostname newname` - Set hostname
- `hostname` - Show current hostname
- `hostname newname` - Set temporary hostname

## Network Testing
- `ping host` - Test connectivity
- `traceroute host` - Trace route to host
- `nslookup domain` - DNS lookup
- `dig domain` - DNS lookup (detailed)
- `netstat -tuln` - Show listening ports
- `ss -tuln` - Show listening ports (modern)
- `telnet host port` - Test port connectivity

## Firewall (firewalld)
- `firewall-cmd --state` - Check firewall status
- `firewall-cmd --get-default-zone` - Show default zone
- `firewall-cmd --list-all` - List all rules in default zone
- `firewall-cmd --list-all-zones` - List all zones
- `firewall-cmd --add-service=http --permanent` - Add service permanently
- `firewall-cmd --add-port=8080/tcp --permanent` - Add port permanently
- `firewall-cmd --reload` - Reload firewall rules
- `firewall-cmd --zone=public --add-interface=eth0` - Add interface to zone

## SSH Configuration
- `/etc/ssh/sshd_config` - SSH daemon configuration
- `systemctl restart sshd` - Restart SSH service
- `ssh-keygen -t rsa` - Generate SSH key pair
- `ssh-copy-id user@host` - Copy public key to remote host
