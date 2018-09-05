SLURM cluster Role 
=======================

Install SLURM cluster [1]. This role has been specifically developed to be used in the INDIGO project.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows.

	# SLURM version to install (in case of RH systems)
	slurm_version: 16.05.8
	# Type of node to install: front or wn
	slurm_type_of_node: front
	# Name of the SLURM server
	slurm_server_name: slurmserver
	# IP address of the SLURM server
	slurm_server_ip: 127.0.0.1
	# Prefix to set to the SLURM working nodes
	slurm_vnode_prefix: vnode-

	# These three values are used to define the WNs
	# Only define one of them. In case of defining more than one
	# they are prioritized in the same order that appears in this file

	# List of the name of the WNs
	slurm_wn_nodenames: []
	# List of IPs of the WNs
	slurm_wn_ips: []
	# Number of WNs
	slurm_wn_num: -1

	# Number of CPUs of the WNs
	slurm_wn_cpus: 1
	# Default user for ssh and slurm management
	slurm_user: slurm
	slurm_uid: "1994"
	# Password used to derive a munge key for authentication between the server and the workers
	slurm_password: hfe1q4ujaucsu913
	# Default user for munge
	munge_user: munge
	munge_uid: "1995"

Example Playbook
----------------

This an example of how to install a Torque/PBS cluster:

    - hosts: server
      roles:
      - { role: 'indigo-dc.slurm', slurm_type_of_node: 'front', slurm_server_ip: '{{ansible_default_ipv4}}', slurm_wn_nodenames: "{{ groups['wns']|map('extract', hostvars, 'ansible_hostname')|list }}" }

    - hosts: wns
      roles:
      - { role: 'indigo-dc.slurm', slurm_type_of_node: 'wn', slurm_server_ip: "{{hostvars['server']['ansible_default_ipv4']}}" }

License
-------

Apache Licence v2 [2]

[1] http://slurm.schedmd.com/

[2] http://www.apache.org/licenses/LICENSE-2.0
