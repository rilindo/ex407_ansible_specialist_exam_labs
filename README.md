# ex407_ansible_specialist_practice_tasks
A repo that contains practice tasks for use in practicing for the Red Hat ex407 exam. Feel free to contribute. No content from the exam will be accepted, though.

# Exam Objectives

The objectives for the Ex 407 exam can be found here:


[Red Hat Certified Specialist in Ansible Automation exam](https://www.redhat.com/en/services/training/ex407-red-hat-certified-specialist-in-ansible-automation-exam)

# Ansible Exam notes

The exam objectives clearly stated that you will be using ansible 2.3. This means some commands in 2.4 and above will not be available in 2.3. Here they are:

## include_tasks

`include_tasks` does not exists in 2.3. Use `include` statement instead

## Tags

If you need to apply a tag to an entire role, you may need to do this instead:

```
roles:
  - { role: webserver, port: 5000, tags: [ 'web', 'foo' ] }
```

[See here for more tag info in 2.3](https://docs.ansible.com/ansible/2.3/playbooks_tags.html)
## vault-id

vault-id is not available as a parameter to use. For now, rely on --ask-vault-pass instead.

## Certificate Errors

Given that 2.3 has fairly outdated certificates, you may get similar to the following  when installing through ansible galaxy:

```
rocinante:ex407_ansible_specialist_practice_tasks rilindo$ ansible-galaxy install https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz validate_certs=False
- downloading role from https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz
 [ERROR]: failed to download the file: Failed to validate the SSL certificate for github.com:443. Make sure your managed
systems have a valid CA certificate installed. You can use validate_certs=False if you do not need to confirm the servers
identity but this is unsafe and not recommended. Paths checked for this platform: /etc/ssl/certs, /etc/ansible,
/usr/local/etc/openssl. The exception msg was: ("bad handshake: Error([('SSL routines', 'SSL3_READ_BYTES', 'tlsv1 alert
protocol version')],)",).
```

Use the -c parameter to ignore the errors (in real life, you should really upgrade Ansible)

```
rocinante:ex407_ansible_specialist_practice_tasks rilindo$ ansible-galaxy install https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz -c
- downloading role from https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz
- extracting v0.1 to /etc/ansible/roles/v0.1
```
# Tips

## Documentation

Most of the documentation can be looked up with `ansible-doc -l`. Since it takes a very long time to pull up, you can pipe the list of modules to file before writing any code. This will save time, as you only need to page through or grep in a text file instead of waiting until `ansible-doc -l` loads.  For example, if you need to look up a mysql_user module, you can do this:

```
stardust:~ rilindo$ ansible-doc -l > /tmp/ansible.txt
stardust:~ rilindo$ grep mysql_user /tmp/ansible.txt 
mysql_user                         Adds or removes a user from a MySQL database.                                                   
proxysql_mysql_users               Adds or removes mysql users from proxysql admin interface.                                      
stardust:~ rilindo$ ansible-doc mysql_user
```

## Installing Roles

You can install from a tarball directly. Here is an example:

```
ansible-galaxy install https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz
```

Currently, ansible-galaxy does *not* install from zip archives.

## Verifying variables

You can print out variables in `name`. This can be useful to verify that you are setting the right variables:

```
- hosts: all
  vars:
    filename: hellworld.txt
  tasks:
    - name: "We are copying file {{ filename }} to the instance"
      copy:
        src: "{{ filename }}"
        dest: "/tmp/{{ filename }}"
```

Note that this would probably not work for loop variables `item` as well as ansible facts.
