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

# Tips

Most of the documentation can be looked up with `ansible-doc -l`. Since it takes a very long time to pull up, you can pipe the list of modules to file before writing any code. This will save time, as you only need to page through or grep in a text file instead of waiting until `ansible-doc -l` loads.  For example, if you need to look up a mysql_user module, you can do this:

```
stardust:~ rilindo$ ansible-doc -l > /tmp/ansible.txt
stardust:~ rilindo$ grep mysql_user /tmp/ansible.txt 
mysql_user                         Adds or removes a user from a MySQL database.                                                   
proxysql_mysql_users               Adds or removes mysql users from proxysql admin interface.                                      
stardust:~ rilindo$ ansible-doc mysql_user
```
