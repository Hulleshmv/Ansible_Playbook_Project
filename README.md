# Ansible_Playbook_Project
create ansible playbook install tomcat

Create a new file called install-tomcat.yml in your Ansible project directory.

Add the following lines to the file to define the hosts and remote user:

---
- name: Install Tomcat on Ubuntu servers
  hosts: ubuntu-servers
  become: yes

  tasks:
  - name: Update package cache and install Java
    apt:
      update_cache: yes
      name: default-jdk

  - name: Download Tomcat archive
    get_url:
      url: "http://mirror.olnevhost.net/pub/apache/tomcat/tomcat-9/v9.0.54/bin/apache-tomcat-9.0.54.tar.gz"
      dest: "/tmp/"

  - name: Extract Tomcat archive
    unarchive:
      src: "/tmp/apache-tomcat-9.0.54.tar.gz"
      dest: "/opt/"
      remote_src: yes

  - name: Set owner and group permissions
    file:
      path: "/opt/apache-tomcat-9.0.54"
      state: directory
      owner: tomcat
      group: tomcat

  - name: Start Tomcat service
    service:
      name: tomcat
      state: started
      enabled: yes


Replace ubuntu-servers with the name of your inventory group containing the Ubuntu servers where you want to install Tomcat. 
You can also specify a different remote user with the remote_user option if necessary

You can now run the playbook with the ansible-playbook command:
ansible-playbook install-tomcat.yml
