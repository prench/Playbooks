# Playbooks
List of  AIX playbooks

* sshd.yml 

  Configure sshd file based on a template and restart the services.

  Following CIS standard, permissions for /etc/ssh/sshd_config should be 600 but that breaks sftp when login with a user different than root

* mksysb.yml

  Create mksysb and store it on a NFS filesystem
  
* hk.yml

  Houskeeping playbook
  
* sudo.yml

  Standardize, install and configure sudoers file.
  
  Old specifications are checked and we can configure which ones we want to remove. Others will go to a file called specs under     /etc/sudoers.d
  
  There is a template stored on the ansible server that is copied over the clients
  
 * ssh folder
  
  Different playbooks to find for a specific key configured on different users, add a new key and remove the old key
