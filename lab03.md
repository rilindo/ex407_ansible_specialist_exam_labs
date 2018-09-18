1. Run an ansible add-hoc command to install httpd on web group
2. Create a playbook to:
  - an encrypted file "secure" with variable "shipclass" set to "truman"
  - Create a file in /var/www/html/shipclass.html using variables in the encrypted file.
  - install on web group
3. Create a playbook:
  - Install MariaDB server with the password of `m4rzN4vy` for root
  - drop anonymous tables.
  - install on database group
  - Add error handling for dropping anonymous tables
4. Create a playbook to:
  - Add `tony`, `steve`, and `nat` as users 
  - Make ure that `tony` can sudo as root.
  - Make sure that `nat` can sudo, but be prompted for the password
  - password are as follows: `st4Rk`, `r0geR5`, `RoMan0v@` 
  - install on bastion host
5. Install torquebox role using the following URL: https://github.com/juggy/torquebox-ansible/archive/master.zip

6. Consolidate 2-4 into a single play
  - Give it the ability to run each playbook using tags
  - Run it against all hosts

7. Create a playbook too
  - get the output of `ps aux`
  - save the output into `/tmp/process_snapshot.txt`
  - run it against all hosts
