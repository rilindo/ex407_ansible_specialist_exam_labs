Note: wrap commands in bash

1. Run an ansible add-hoc command to install elinks
2. Create a file "/tmp/hosts.txt" with the OS, IP and Hostname.
3. Create a playbook to:
  - Add `donnager.example.com` to /etc/hosts set to the ip of the host
    - Executed with tag `updated webhost`
  - Install apache and ensure:
    - It is running
    - It will start on boot
    - Set host to `donnager.example.com`
    - Executed when you run tag `webserver`
    - Create a site:
      - Added an index.html in /var/www/html with the follow text
         - Welcome to the Martian Marine Web Site
      - Password protect page with the following username and password
         - bobbie - cucUmb3rS4nwitch
   - Applied to hosts in group `web`
   - Password to protect info will be `3arthSucks`
4. Set default ansible forks to 20.
5. Set default ansible timeout to 120 seconds.
