# ex407_ansible_specialist_practice_tasks
A repo that contains practice tasks for use in practicing for the Red Hat ex407 exam. Feel free to contribute. No content from the exam will be accepted, though.

# Exam Objectives

The objectives for the Ex 407 exam can be found here:


[Red Hat Certified Specialist in Ansible Automation exam](https://www.redhat.com/en/services/training/ex407-red-hat-certified-specialist-in-ansible-automation-exam)

# Ansible Exam notes

The exam objecties explicitely stated that you will be using ansible 2.3. This means some commands in 2.4 and above will not be available in 2.3. Here they are:

## include_tasks

`include_tasks` does not exists in 2.3. Use `include` statement instead

## vault-id

vault-id is not available as a parameter to use. For now, rely on --ask-vault-pass instead.
