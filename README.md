# Create User Using Ansible PlayBook


In this guide I will explore how to automate the process of Linux user creation using Ansible Playbook.

Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. It runs on many Unix-like systems.

------
### Prerequisites

1. 2 Instance needed, 1 for Ansible configuration and the other 1 for remote server configuration
2. Install and configure Ansible in the main Instance and set up the inventry file remore ( client ) server.
3. Test the Inventry file ( remote servers ) using the ping commant.


-----
### Create User with Password using Ansible Playbook

Setup your playbook as followed. I have created a directory in the main instance called "create-user" and created a YML file inside the directory.  Here my YML file is user-creation.yml.

~~~
mkdir create-user && cd create-user

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
       src: "./.ssh/"
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


Here I am using the AWS EC2 instance and I have a PEM file for the ec2-user. The ec2-user PEM file is copied to the new user directory for login with the same key from the outside to the server as the new username "abhiraj". Also, I have set up the password for the new user. The password is used to log in to the user inside the instance, The pasword login not enabled in the server, so the password login didn't work from the outside.

Run the command first as below for the playbook syntax check. I have attached the output screenshot for reference. 

~~~
ansible-playbook -i hosts  user-creation.yml --syntax-check
~~~


![Screenshot from 2022-04-20 00-56-57](https://user-images.githubusercontent.com/103326353/164080639-b0c8b32e-1167-48d7-bb60-58169d0e34a9.png)



To run this playbook, run the command below. This will input the user password variable that will be used by our playbook. The user password is "qwerty123!"

~~~
ansible-playbook -i hosts  user-creation.yml --extra-vars userpassword=qwerty123!
~~~


Next, Check the user creation and logins are working or not.

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










