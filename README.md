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
 
# 2. Flexible Storage Backend (LVM)
** Objective ** Deploy a scalable storage layer for web content to allow online resizing.
** Implementation **
- Initialized physical volume on '/dev/sdb'.
- Created Volume Group 'vg_web' and a 1GB Logical Volume 'lv_webdata'.
- Formatted volume with XFS filesystem.
- Configured persistent mount to '/web_data' via '/etc/fstab' using UUID.
** Verification **
- Confirmed LVM structure with 'lvs'.
- Verified mount status with 'df -h'.

## 3. Security Hardening (Firewalld & SELinux)
** Objective ** Secure the network perimeter and apply Mandatory Access Control (MAC) policies.
** Implementation **
- Configured 'firewalld' to permanently allow 'http' service traffic.
- Defined a custom SELinux policy rule for the non-standard '/web_data' path using 'semanage'.
- Labeled the storage layer with 'httpd_sys_content_t' to allow the web daemon access.
** Verification **
- Confirmed firewall rules with 'firewall-cmd --list-all'.
- Verified security contexts with 'ls -Zd /web_data'.

## 4. Containerized Service Deployment (Podman & Systemd)
** Objective ** Deploy a highly available, containerized web service integrated with systemd.
** Implementation **
- Deployed an Apache 2.4 container using 'podman run'.
- Mounted persistent LVM storage from '/web_data' to the container's web root using the ':Z' SELinux flag for security.
- Generated a systemd unit file using 'podman generate systemd' to manage the container lifecycle.
- Enabled the service for persistence across reboots using 'systemctl --user enable'.
** Verification **
- Confirmed container status with 'podman ps'.
- Verified web access via 'curl http://localhost:8080'.

## 5.Automation & Recovery
** Objective ** Automate system healthy monitoring and data redundacy.
** Implementation ** 
-- Developed a Bash script `web_maintenance.sh` to perform automated health checks.
- Integrated a conditional logic `if` statement to detect service failure and trigger self-healing (systemd restart).
- Automated a daily backup routine using `tar` with Gzip compression, targeting the persistent LVM storage layer.
- Implemented dynamic naming conventions using shell variables and command substitution (`date`).
** Verification **
- Successfully executed script manually to confirm backup generation in `/root/backups`.
- Confirmed "self-healing" by stopping the container and verifying the script restarted it.

