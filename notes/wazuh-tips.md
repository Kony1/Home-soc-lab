# Wazuh Tips & Tricks

## Real-time Monitoring
By default, Wazuh scans files periodically. To enable real-time alerts for `/etc`:
1. Edit `/var/ossec/etc/ossec.conf`
2. Find `<directories check_all="yes" realtime="yes">/etc</directories>`
3. Restart manager: `systemctl restart wazuh-manager`

## Connectivity
- Agent to Manager port: `1514 (TCP/UDP)`
- Registration port: `1515 (TCP)`
