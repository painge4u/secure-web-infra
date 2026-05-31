# secure-web-infra

## 1.Network backbond configuration
** objective ** Establish persistent static network identity for the production env
** Implementation**
-Identified interface using 'nmcli dev status'.
-Configured static ipv4 address '192.168.0.100/24' and gateway '192.168.0.1'.
-Set the method to 'manual' to ensure persistence across reboots.
-Defined system hostname as 'web-prod.example.com' using 'hostnamectl'.
**Verification**
-Verified ip assignment with 'ip addr show'.
-Confirmed hostname change with 'hostname'.
