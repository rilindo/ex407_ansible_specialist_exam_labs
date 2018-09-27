1. Create playbook to:
   - Install NFS
   - Export /home/users
   - Deployed to hosts in group `bastion`
2. Create playbook to:
   - Install mysql client and maria server and start it up.
   - Deployed to hosts in group `database`

3. Install lighttpd
   - Using the role located at https://github.com/monzell/ansible-lighttpd/archive/v0.1.tar.gz
   - Install on `web` group

4. Install CA certs from https://www.geotrust.com/resources/root-certificates/ into /etc/pki/ca-trust/source/anchors/ and run update-ca-trust.
   - install on `web` group

5. Consolidate 1-4 into a single play
   - each role should be tagged
   - It should with all hosts in inventory

6. Write a  play for each of the following modules:
   - ping
   - setup
   - yum
   - service
   - user
   - copy
   - file
   - git
   - lineinfile
   - htpasswd
   - script
   - debug
