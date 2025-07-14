# Foreman Installation with DHCP/DNS/TFTP and discovery image
**Fully installation using ansible - RHEL9 Foreman Guide**
- Install Forman with katello, puppet, and discovery plugin.
- Autoconfig DCHP/DNS and TFTP.
- Be 100% ready to use: deploy servers via PXE (using discovery image)

For this, the follow Server is using:

  -  Vmware WorkStation Pro 17.6.1 (personal use | free license)
  -  Foreman 3.14
  -  Katello 4.15
  -  Puppet 8
  -  Ansible 8.7.0 (core 2.15.13)
  -  CentOS 9.5 with:

![image](https://github.com/user-attachments/assets/88b79f6d-0263-4988-ba0a-3ead9b71eb90)

**ATTENTION**:
Foreman requires at least 20GB of RAM and 4GB of swap. To perform a standard installation, set the value of the "tuning" variable in Ansible for "deployment" to "default" or higher.

https://docs.theforeman.org/3.14/Installing_Server/index-katello.html#system-requirements_foreman
https://docs.theforeman.org/3.14/Installing_Server/index-katello.html#tuning-with-predefined-profiles_foreman

#Step 0: The virtual machine configuration

Set to use 2 network interfaces: 1 for public network and another for deployment 
![image](https://github.com/user-attachments/assets/6830e949-c907-444d-8ef5-e6590847b526)



#Step 1: Add those repositories:

# Installing Repositories for Foreman and Katello

Run the following commands one by one on your EL9 (Enterprise Linux 9) system:

1. **Add the Foreman repository**:
   ```bash
   sudo dnf install https://yum.theforeman.org/releases/3.14/el9/x86_64/foreman-release.rpm -y
2. **Add the Katello repository**:
   ```bash
   sudo dnf install https://yum.theforeman.org/katello/4.15/katello/el9/x86_64/katello-repos-latest.rpm -y
3. **Add the Puppet 8 repository**:
   ```bash
   sudo dnf install https://yum.puppet.com/puppet8-release-el-9.noarch.rpm -y
4. **Install Ansible**
   ```bash
   sudo dnf install python3-pip -y
   
   pip3 install ansible==8.7.0
4. **Upgrade all system packages**:
   ```bash
   sudo dnf upgrade -y

# Ansible configuration

  - Download the repository:
    ```bash
    git clone https://github.com/enemy100/Foreman-deploy.git
  - Go to the directory:
    ```bash
    cd Foreman-deploy/
![image](https://github.com/user-attachments/assets/7212c127-1f19-48fe-8c06-f972d278c0e2)


Adjust the variables according with your environment

1. **Host setup**

  Ansible will configure the network interface, set the hostname, manage the firewall and SELinux (disabling them), and create some directories.
  Adjust the variables below according to your environment.

![image](https://github.com/user-attachments/assets/1625124c-17ca-4106-a083-316953e88f78)


for example, for the environment lab, we have two interfaces: ens160 and ens224, where the 2nd is dedicate for provisioning
![image](https://github.com/user-attachments/assets/e2476c48-e772-4438-8a86-630db4c4c926)


2. **Deploy Foreman with Katello and these foreman-proxy: DNS/TFTP/DHCP**
  
  **Execute the command**:
  ```bash
  ansible-playbook -i inventory.yml site.yml --ask-become-pass
  ```
The ansible will do:

  - Adjust server name fqdn and add at /etc/hosts
  - Configure the network interface for provisioning
  - Disable firewalld and Selinux
  - Create pulp/PostgreSQL/puppetlabs directories
  - Install Foreman/Katello
  - Run a Foreman installer command
  - Download discovery kernel image


   ![image](https://github.com/user-attachments/assets/1db3fa2b-7550-4854-93b3-fb24c2a2be63)
![image](https://github.com/user-attachments/assets/8b7bd19d-11ee-4e7b-aee1-b7c068b4fe02)

3. **Using the PXE Discovery image**

Since DHCP and TFTP are now managed by katello, with one click we can create default templates for boot (or do it via the command line using hammer, which in this case, would also be possible to add to the ansible playbook. For example purposes, let's create the template like this:

Go to Host -> Provisioning Setup -> Provisioning Templates.
Click on "Build Pxe default". It will create the templates for booting via BIOS and UEFI.
![image](https://github.com/user-attachments/assets/6367b2f6-6144-4758-aae6-4a6888a1b2a8)

Start the client machine (using the host-only network)

![image](https://github.com/user-attachments/assets/a7608e44-0c96-4e09-a361-c79c2179a30f)

From here, the virtual machine is ready for deployment!
![image](https://github.com/user-attachments/assets/4e904b88-b039-44fa-9f6f-9f7663f684bb)


# Ideas
- We can use dell ansible modules to configure iDRACS and delivery the host to Foreman
- We can conect to Vmware/Libvirt, Openstack, etc via ansible.
- Create a Disaster/recovery solution for Foreman with Puppet, Ansible, Pacemaker, DB HA, etc...












