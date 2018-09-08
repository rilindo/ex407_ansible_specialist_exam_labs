Note: wrap ad-hoc commands in bash

1. Run an ansible add-hoc command to install elinks
2. Create a file "/tmp/hosts.txt" with the OS, IP and Hostname.
3. Create a playbook to:
  - Add `donnager.example.com` to /etc/hosts set to the ip of the host
  - Install httpd and ensure:
    - It is running
    - It will start on boot
    - Set host to `donnager.example.com`
    - Executed when you run tag `webserver`
    - Create a site:
      - Added an index.html in /var/www/donnager.example.com/public_html/ with the follow text
         - Welcome to the Martian Marine Web Site
      - Password protect page with the following username and password
         - bobbie - cucUmb3rS4nwitch
   - Applied to hosts in group `web`

Here is a virtualhost to use for the site

```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/donnager.example.com/public_html
    ErrorLog /var/log/httpd/donnager.example.com.error.log
    CustomLog /var/log/httpd/donnager.example.com.access.log combined
</VirtualHost>
```

And the default site:
```
<VirtualHost _default_:*>
    DocumentRoot "/www/default"
</VirtualHost>
```
4. Set default ansible forks to 20.
5. Set default ansible timeout to 120 seconds.
6. Write a playbook to:
  - apply all security updates
  - reboot all instances and monitor until they come back up
