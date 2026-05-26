Step 1: Install Ansible

Run:

sudo apt update
sudo apt install ansible -y

It may ask for password. Type your Ubuntu password. The password may not visibly appear while typing. That is normal.

Step 2: Check Ansible version

Run:

ansible --version

Expected output:

ansible [core ...]

If you see version details, Ansible is installed.

Step 3: Create experiment folder

Run:

mkdir exp10
cd exp10

Now your terminal should look like:

~/exp10$
Step 4: Create inventory file

Run:

nano hosts.ini

Paste this:

[webservers]
localhost ansible_connection=local

Save in nano:

Ctrl + O
Enter
Ctrl + X

Meaning:

Ctrl + O = save
Enter = confirm filename
Ctrl + X = exit
Step 5: Create playbook file

Run:

nano install_nginx.yml

Paste this exactly:

---
- name: Install and Start Nginx
  hosts: webservers
  become: yes

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx service
      service:
        name: nginx
        state: started
        enabled: yes

Save:

Ctrl + O
Enter
Ctrl + X

YAML is allergic to bad spacing, so keep indentation exactly.

Step 6: Check files are created

Run:

ls

You should see:

hosts.ini  install_nginx.yml
Step 7: Run the Ansible playbook

Run:

ansible-playbook -i hosts.ini install_nginx.yml

Expected output should show tasks like:

TASK [Update apt cache]
TASK [Install Nginx]
TASK [Start Nginx service]
PLAY RECAP
localhost : ok=...

If it asks for sudo password, use:

ansible-playbook -i hosts.ini install_nginx.yml --ask-become-pass

Then enter your Ubuntu password.

Step 8: Verify Nginx is running

Run:

systemctl status nginx

Expected:

active (running)

If systemctl gives issues in WSL, use:

service nginx status

Expected:

nginx is running
Step 9: Open browser

Open browser on Windows and visit:

http://localhost

Expected output:

Welcome to nginx!

That is your final practical output.
