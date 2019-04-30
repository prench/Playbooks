# Playbooks
List of playbooks

* sshd.yml 

Configure sshd file based on a template and restart the services.

Following CIS standard, permissions for /etc/ssh/sshd_config should be 600 but that breaks sftp when login with a user different than root
