fail2ban
=========

A simple role to add custom fail2ban jails. 

Role Variables
--------------

You can adjust a few variables to customize this rule, such as:  

- **customban:** Set this to false if you dont want use a custom template and just install fail2ban  
- **customfile:** the name of the customfile, by default it will be copied to "/etc/fail2ban/jail.d/"  
- **packs:** is the packages to be installed with this role  
- **dest:** this is the destination account for alerts  
- **sender:** the sender address of alerts  

- **services:** Is used on template custom.j2 to build the fail2ban jail file.  
This role will be called on your system by name-custom, you can list jails using client  
e.g:  

```
$fail2ban-client status
```

You need to define all variables on services folowed by:  

* **name:** the name of the rule  
* **filter:** define a filter to use. (List filters on /etc/fail2ban/filter.d )  
* **majorname:** the description name for this jail  
* **port:** which port you are using  
* **protocol:** by default is tcp  
* **retry:** max number of retry  
* **bantime:** how many time to be banned in seconds  


Dependencies
------------

None

Example Playbook
----------------

You can build a playbook for add a single jail or multiple. Here is a sample to add a simple jail  
filter.

```
---

- name: " Deploy | Running isca0.fail2ban role "
  hosts: somehost
  become: yes
  remote_user: myuser

  vars:
    trusted:
      - "127.0.0.1/8"
      - "10.120.30.0/27"
    services:
      - name: ssh
        filter: sshd
        majorname: SSH
        port: 1022
        protocol: tcp
        dest: root
        email: root@localhost
        retry: 3
        bantime: 1800
 
  roles:
    - fail2ban

```

You can also add multiple jail at same time by doing like this:

```
---

- name: " Deploy | Running isca0.fail2ban role "
  hosts: somehost
  become: yes
  remote_user: myuser

  vars:
    trusted:
      - "127.0.0.1/8"
      - "10.120.30.0/27"
    services:
      - { name: ssh, filter: sshd, majorname: SSH, port: 1022, protocol: tcp, retry: 3, bantime: 1800 }
      - { name: postfix, filter: postfix, majorname: POSTFIX, port: "smtp,465,submission", protocol: tcp, retry: 3, bantime: 60 }
 
  roles:
    - fail2ban

```


License
-------

LGPL-3.0

Author Information
------------------

This role was create in 2017 by [isca](https://isca.space)
