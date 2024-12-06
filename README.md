# Foreman-deploy
Fully foreman installation (DHCP/DNS/TFTP) using ansible

For this, the follow Server is using:

  -  Vmware WorkStation Pro 17.6.1 (personal use | free license)
  -  Foreman 3.10
  -  Katello 4.12
  -  Puppet 7
  -  Ansible 8.7.0
  -  CentOS 9.5 with:

![image](https://github.com/user-attachments/assets/88b79f6d-0263-4988-ba0a-3ead9b71eb90)

#Step 0: The virtual machine configuration

Set to use 2 network interfaces: 1 for public network and another for deployment 
![image](https://github.com/user-attachments/assets/6830e949-c907-444d-8ef5-e6590847b526)



#Step 1: Add those repositories:

# Installing Repositories for Foreman and Katello

Run the following commands one by one on your EL9 (Enterprise Linux 9) system:

1. **Add the Foreman repository**:
   ```bash
   sudo dnf install https://yum.theforeman.org/releases/3.10/el9/x86_64/foreman-release.rpm -y
2. **Add the Katello repository**:
   ```bash
   sudo dnf install https://yum.theforeman.org/katello/4.12/katello/el9/x86_64/katello-repos-latest.rpm -y
3. **Add the Puppet 7 repository**:
   ```bash
   sudo dnf install https://yum.puppet.com/puppet7-release-el-9.noarch.rpm -y
4. **Install Ansible**
   ```bash
   sudo dnf install python3-pip -y
   pip3 install ansible==8.7.0
4. **Upgrade all system packages**:
   ```bash
   sudo dnf upgrade -y

# Ansible configuration

Adjust the variables according with your environment

1. **Host setup**

  Ansible will config the network interface, set a hostname and create, disable firewall and selinux, and create some dirs
  adjust the variables below acording with your envirnoment

  ![image](https://github.com/user-attachments/assets/964ed7a7-f24a-4aff-95f5-ebc648facded)


2. **Deploy Foreman with Katello and those foreman-proxy: DNS/TFTP/DHCP**
  
  **Execute the command**:
  ```bash
  ansible-playbook site.yml







