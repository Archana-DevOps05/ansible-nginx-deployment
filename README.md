## Automating Server Configuration with Ansible

This guide will help you automate server configuration using Ansible to install Nginx, deploy a sample web page, and ensure the service is running.

## Prerequisites
A Linux-based remote server (AWS).

Ansible installed on your local machine.

SSH access to the remote server.

##  1. Install Ansible and Set Up SSH Access
sudo apt update

sudo apt install -y ansible

## Set Up SSH Access to Remote Server

Ensure you can SSH into the server without a password:
ssh-keygen -t rsa -b 2048

ssh-copy-id user@your-server-ip
Replace user with your  username (e.g., ubuntu) and your-server-ip with the server's IP address.

Verify SSH access:
ssh user@your-server-ip

## 2. Create a Directory 
mkdir ansible-project

cd ansible-project

## 3. Write an Ansible Playbook
nano setup.yml
   - name: Setup Nginx Web Server
  hosts: web_servers
  become: yes  # Run as root
  tasks:

    - name: Install Nginx
      apt:
        name: nginx
        state: present
      when: ansible_os_family == "Debian"

    - name: Start and Enable Nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Deploy a Sample Web Page
      copy:
        dest: /var/www/html/index.html
        content: "<h1>Welcome to the Ansible Automated Server! Your server is now fully configured, optimized, and ready to handle your web applications seamlessly.</h1>"

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted

## 4. Create  inventory.ini file
    nano inventory.ini

      [web_servers]
your-server-ip ansible_user=ubuntu

[Replace your-server-ip with actual ip adderess]

## 5. Run the Ansible Playbook
  ansible-playbook -i inventory.ini setup.yml

  ## This will:
✅ Install Nginx
✅ Deploy a sample web page
✅ Ensure Nginx is running

## 6. Verify the Setup
  Open your browser and visit:

  http://your-server-ip
  
