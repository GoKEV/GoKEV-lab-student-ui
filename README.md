[![GoKEV](http://GoKEV.com/GoKEV200.png)](http://GoKEV.com/)

<div style="position: absolute; top: 40px; left: 200px;">

# GoKEV-lab-student-ui

This project is an Ansible role to deploy one or many EC2 instances for lab purposes

<pre>
##      SOME OPTIONS FOR SIZING (MAY NOT BE CURRENT INFO):
#
#               vCPU    ECU     Memory          Storage (GB)		Ambiguous reference for Kev 
#								    (not a reference for AWS EC2 pricing)
#  t2.small     1       Var     2 GiB           EBS Only                0.023 units of ambiguity
#  t2.medium    2       Var     4 GiB           EBS Only                0.0464 units of ambiguity
#  t2.large     2       Var     8 GiB           EBS Only                0.0928 units of ambiguity
#  t2.xlarge    4       Var     16 GiB          EBS Only                0.1856 units of ambiguity
#  t2.2xlarge   8       Var     32 GiB          EBS Only                0.3712 units of ambiguity
#
#  r5d.4xlarge  16      71      128 GiB         2 x 300 NVMe SSD        1.152 units of ambiguity
#  r5.4xlarge   16      71      128 GiB         EBS Only                1.008 units of ambiguity
#  r4.8xlarge   32      99      244 GiB         EBS Only                2.128 units of ambiguity
#
</pre>


## Here's an example of how you could launch this role:
<pre>
ansible-playbook -i ec2.hosts GoKEV-lab-builder.yml
</pre>

## Example Playbook called GoKEV-lab-builder.yml:

<pre>
---
- name: Build an EC2 instance
  hosts: localhost
  gather_facts: no

  vars:
    user_srvname: GoKEV Server Name Field
    user_sshkey: my-private-key
    user_instype: t2.small
    user_image: ami-26ebbc5c
    user_vpc: vpc-a12345678
    user_subnet: subnet-12345678
    user_secgrp: security-group-name
    user_awskey: ABCDEFGHIJKLMNOPQRST
    user_awssec: abcdefghijklmnopqrstuvwxyz0123456789
    user_region: us-east-1
    user_tags:
      Name: "{{ user_srvname }}"
      Event: Some Workshop Event
      Owner: GoKEV
      Contact: "kev@redhat.com"
      Application: AnsibleRED

  roles:
    - GoKEV.lab-provision

- hosts: newec2
  roles:
    - GoKEV.lab-installtower
    - GoKEV.lab-configtower
    - GoKEV.lab-deck
    - GoKEV.lab-docker-nodes
    - GoKEV.lab-student-ui
</pre>

## With an inventory file called ec2.hosts:
<pre>
[local]
localhost

[newec2:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=/path-to-my/aws/key-file.pem

</pre>
## With a requirements.yml that looks as such:

<pre>
---
- name: GoKEV.lab-provision
  version: master
  src: https://github.com/GoKEV/GoKEV-lab-provision.git

- name: GoKEV.lab-installtower
  version: master
  src: https://github.com/GoKEV/GoKEV-lab-installtower.git

- name: GoKEV.lab-configtower
  version: master
  src: https://github.com/GoKEV/GoKEV-lab-configtower.git

- name: GoKEV.lab-deck
  version: master
  src: https://github.com/GoKEV/GoKEV-lab-deck.git

- name: GoKEV.lab-docker-nodes
  version: master
  src: https://github.com/GoKEV/GoKEV-lab-docker-nodes.git

- name: GoKEV.lab-student-ui
  version: master
  src: https://github.com/GoKEV/GoKEV-lab-student-ui.git

</pre>


## Troubleshooting & Improvements

- Not enough testing yet

## Notes

  - Not enough testing yet

## Author

This project was created in 2018 by [Kevin Holmes](http://GoKEV.com/).


