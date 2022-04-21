# Create User Using Ansible PlayBook


In this guide I will explore how to automate the process of Linux user creation using Ansible Playbook.

Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. It runs on many Unix-like systems.

------
### Prerequisites

1. 2 Instance needed, 1 for Ansible Manager node and the other 1 for client node
2. Install and configure Ansible in the Manager node and set up the inventry file for client node in the Manager server .
3. Test the Inventry file ( client node ) using the ping commant.


-----
### Installing ansible on manager node.

You can use the below command to install the Ansible in the Manager node.
~~~
sudo amazon-linux-extras install ansible2 -y
~~~

Once the installation completed. Please check the version throught the below command. 
~~~
ansible --version

ansible 2.9.23
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/ec2-user/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.18 (default, Jun 10 2021, 00:11:02) [GCC 7.3.1 20180712 (Red Hat 7.3.1-13)]
~~~

-----
### Adding client node in hosts of manager node

 Here I have created a working directory in the main instance ( Managed node ) called "create-user" and I have created a file "hosts" under the create-user directory.

~~~
mkdir create-user && cd create-user

touch hosts
~~~

Then I have added the client node detailsin the hosts ( invetry ) file. The details are username, port number, keyfile and IP address. The dtails are added to below sniiptthis. 

~~~
[amazon]                                                                                        
<server IP> ansible_user="ec2-user" ansible_port=22 ansible_ssh_private_key_file="abhira.pem"
~~~

Abhiraj.pem is the private key that im using for accessing the client node. [amazon] is the group name i have given to the client nodes.

Note: please hold the pem and hosts file in the working directory. my working directory is create-user

-------
### Create User with Password using Ansible Playbook

Setup your playbook as followed. I have created created a YML file inside the working directory.  Here my YML file is user-creation.yml.

~~~
vi user-creation.yml
~~~

Then copy-paste the below content into the YML  file. 

~~~

---

- name: "User Creation For Devops Engineer"
  become: true
  hosts: amazon
  tasks:

    - name: "New User Add Remote Server"
      ansible.builtin.user:
        name: abhiraj
        comment: Devops-Enginner
        uid: 10143
        group: ec2-user
        createhome: yes
        home: /home/abhiraj
        shell: /bin/bash
        password: "{{ userpassword|password_hash('sha512') }}"
  
    - name: "Public key copy to new user"
      copy:
       src: "../.ssh/"
       dest: "/home/abhiraj/.ssh/"
~~~

Let's cover that task definition:

1. ansible.builtin.user :  Its used for Manage user accounts.
2. name: Set the username ( here my user name is abhiraj ).
3. comment: Optionally sets the description of user account.
4. groups: Assign a groups. Here I add this user to groups "ec2-user".
5. createhome: Yes to create a home directory, no to not create one
6. home: If you don't want to use the default location, set it here
7. shell: You can define any shell. The default is /bin/bash. I just re-iterate the default here.
8. password: Settup encripted password for user


Here I am using the AWS EC2 instance and I have a PEM file for the ec2-user. The ec2-user PEM file is copied to the new user directory for login with the same key from the server outside as the new username "abhiraj". Also, I have set up the password for the new user. The password is used to log in to the user inside the instance, The pasword login not enabled in the server, so the password login didn't work from the outside.

Run the command first as below for the playbook syntax check. I have attached the output screenshot for reference. 

~~~
ansible-playbook -i hosts  user-creation.yml --syntax-check
~~~


![Screenshot from 2022-04-20 00-56-57](https://user-images.githubusercontent.com/103326353/164080639-b0c8b32e-1167-48d7-bb60-58169d0e34a9.png)



To run this playbook, run the command below. This will input the user password variable that will be used by our playbook. The user password is "qwerty123!"

~~~
ansible-playbook -i hosts  user-creation.yml --extra-vars userpassword=qwerty123!
~~~

-----
### Check the user creation and user logins 

Please login to the remote server ( client server ) with the ec2-user key ( pem ) file.  I have attached the screenshot for the reference.



![Screenshot from 2022-04-20 01-07-37](https://user-images.githubusercontent.com/103326353/164082309-d18fe4fb-6f6c-441d-ab63-91b6c1cd09c2.png)


Here the logins are working amd the user creation completed.


---
## Conclusion

In this tutorial, I have shown you create a user with Ansible Playbook. This is an easier way to set up a user in the linux machine using automation.


---
### ⚙️ Connect with Me

 <p align="center">
<a href="mailto:aparthan275@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://www.instagram.com/_r.e.b.e.l.z_33/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/abhiraj-parthan-82038b191"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
<a href="https://www.wppredirect.tk/go/?p=918893532145&m=Abhiraj%20Parthan."><img src="https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white"/></a>
  </a></p>
</div>










