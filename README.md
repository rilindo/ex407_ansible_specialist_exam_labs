# ex407_ansible_specialist_practice_tasks
A repo that contains practice tasks for use in practicing for the Red Hat ex407 exam. Feel free to contribute. No content from the exam will be accepted, though.

# Instructions

Each practice file is a session. You should complete everything in each session in just about under an hour (ideally, around 30 minutes). Since the exam is 4 hours, these sessions should give you enough practice to finish all the tasks in the exam while still have time to review and refine.

You should have at least 2 servers to work with - your control station (where you run your ansible commands from) and remote server. I recommend you should at least have 4 instances to work with and then you can setup your inventory as follows (ips are not real, of course):

```
[database]
10.20.30.40

[web]
10.40.80.120

[bastion]
10.60.120.180

[app]
10.100.200.10
```

You should have another one for running Ansible Tower as well.

If you want to practice using a cloud provider, there are many that you can use that are inexpensive, such as Digital Ocean. Feel free to use [this link](https://m.do.co/c/75f7a0300020) to sign up for an account (as well as support this effort by giving me free credits. :) )
 
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

Given that 2.3 has fairly outdated certificates, you may get an error similar to the following  when installing through ansible galaxy:

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
### CLI
Most of the documentation can be looked up with `ansible-doc -l`. Since it takes a very long time to pull up, you can pipe the list of modules to file before writing any code. This will save time, as you only need to page through or grep in a text file instead of waiting until `ansible-doc -l` loads.  For example, if you need to look up a mysql_user module, you can do this:

```
stardust:~ rilindo$ ansible-doc -l > /tmp/ansible.txt
stardust:~ rilindo$ grep mysql_user /tmp/ansible.txt 
mysql_user                         Adds or removes a user from a MySQL database.                                                   
proxysql_mysql_users               Adds or removes mysql users from proxysql admin interface.                                      
stardust:~ rilindo$ ansible-doc mysql_user
```
### Loops and Other documentation

It may be possible to install additional documentation by running:

```
sudo yum -y install ansible-doc
```


This will let you install ansible web site documentation:
```
[centos@centos-s-1vcpu-1gb-sfo2-02 ~]$ sudo yum -y install ansible-doc
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.oit.uci.edu
 * extras: mirror.sjc02.svwh.net
 * updates: mirror.san.fastserv.com
Resolving Dependencies
--> Running transaction check
---> Package ansible-doc.noarch 0:2.4.2.0-2.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================================
 Package                                    Arch                                  Version                                         Repository                             Size
==============================================================================================================================================================================
Installing:
 ansible-doc                                noarch                                2.4.2.0-2.el7                                   extras                                763 k

Transaction Summary
==============================================================================================================================================================================
Install  1 Package

Total download size: 763 k
Installed size: 1.9 M
Downloading packages:
ansible-doc-2.4.2.0-2.el7.noarch.rpm                                                                                                                   | 763 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : ansible-doc-2.4.2.0-2.el7.noarch                                                                                                                           1/1 
  Verifying  : ansible-doc-2.4.2.0-2.el7.noarch                                                                                                                           1/1 

Installed:
  ansible-doc.noarch 0:2.4.2.0-2.el7                                                                                                                                          

Complete!
[centos@centos-s-1vcpu-1gb-sfo2-02 ~]$ rpm -ql ansible-doc
/usr/share/doc/ansible-doc-2.4.2.0
/usr/share/doc/ansible-doc-2.4.2.0/rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/YAMLSyntax.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/become.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/command_line_tools.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/committer_guidelines.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/common_return_values.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community
/usr/share/doc/ansible-doc-2.4.2.0/rst/community.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/code_of_conduct.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/communication.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/development_process.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/how_can_I_help.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/index.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/maintainers.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/other_tools_and_programs.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/reporting_bugs_and_features.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/community/triage_process.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/conf.py
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/Makefile
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_api.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_core.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_inventory.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_module_utilities.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_best_practices.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_checklist.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_documenting.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_general.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_general_OLD.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_general_windows.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_modules_in_groups.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_plugins.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_program_flow_modules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_python3.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/developing_rebasing.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/index.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/overview_architecture.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/repomerge.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/Makefile
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/__init__.py
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/breadcrumbs.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/footer.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/layout.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/layout_old.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/search.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/searchbox.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/css
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/css/badge_only.css
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/css/old-theme.css
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/css/theme.css
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/font
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/font/fontawesome_webfont.eot
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/font/fontawesome_webfont.svg
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/font/fontawesome_webfont.ttf
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/font/fontawesome_webfont.woff
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/js
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/static/js/theme.js
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/theme.conf
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/_themes/srtd/versions.html
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/basic_rules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/conf.py
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/grammar_punctuation.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/images
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/images/commas-matter-2.jpg
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/images/commas-matter.jpg
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/images/hyphen-funny.jpg
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/images/thenvsthan.jpg
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/index.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/resources.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/spelling_word_choice.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/trademarks.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/voice_style.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/style_guide/why_use.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/compile
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/compile/index.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/ansible-doc.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/ansible-var-precedence-check.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/azure-requirements.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/boilerplate.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/configure-remoting-ps1.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/empty-init.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/import.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/integration-aliases.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/line-endings.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-basestring.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-dict-iteritems.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-dict-iterkeys.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-dict-itervalues.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-get-exception.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-smart-quotes.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-underscore-variable.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-unicode-literals.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/no-wildcard-import.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/pep8.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/pylint-ansible-test.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/pylint.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/replace-urlopen.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/required-and-default-attributes.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/rstcheck.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/sanity-docs.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/shebang.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/shellcheck.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/test-constraints.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/use-compat-six.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/validate-modules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing/sanity/yamllint.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_compile.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_httptester.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_integration.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_integration_legacy.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_pep8.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_running_locally.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_sanity.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_units.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_units_modules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/dev_guide/testing_validate-modules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/faq.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/galaxy.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/glossary.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_aws.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_azure.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_cloudstack.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_docker.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_gce.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_kubernetes.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_packet.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_rax.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_rolling_upgrade.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guide_vagrant.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/guides.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/index.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_adhoc.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_bsd.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_configuration.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_dynamic_inventory.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_getting_started.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_installation.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_inventory.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_networking.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_patterns.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/intro_windows.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/modules
/usr/share/doc/ansible-doc-2.4.2.0/rst/modules.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/modules/.gitdir
/usr/share/doc/ansible-doc-2.4.2.0/rst/modules_intro.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/modules_support.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/network_debug_troubleshooting.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbook_pathing.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_acceleration.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_advanced_syntax.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_async.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_best_practices.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_blocks.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_checkmode.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_conditionals.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_debugger.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_delegation.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_environment.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_error_handling.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_filters.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_filters_ipaddr.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_intro.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_lookups.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_loops.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_prompts.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_python_version.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_reuse.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_reuse_includes.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_reuse_roles.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_roles.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_special_topics.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_startnstep.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_strategies.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_tags.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_templating.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_tests.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_variables.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/playbooks_vault.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/porting_guide_2.0.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/porting_guide_2.3.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/porting_guide_2.4.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/porting_guides.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/python_3_support.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/quickstart.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/release_and_maintenance.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/roadmap
/usr/share/doc/ansible-doc-2.4.2.0/rst/roadmap/ROADMAP_2_1.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/roadmap/ROADMAP_2_2.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/roadmap/ROADMAP_2_3.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/roadmap/ROADMAP_2_4.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/test_strategies.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/tower.rst
/usr/share/doc/ansible-doc-2.4.2.0/rst/vault.rst
```

For jinja2 documentation, you can also look it up locally with elinks:
```
elinks file:///usr/share/doc/python-jinja2-2.7.2/html/templates.html
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
