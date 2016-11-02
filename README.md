SLURM cluster Role 
=======================

Install SLURM cluster [1]. This role has been specifically developed to be used in the INDIGO project.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows.

	# SLURM version to install (in case of RH systems)
	slurm_version: 14.11.3
	# Type of node to install: front or wn
	slurm_type_of_node: front
	# Name of the SLURM server
	slurm_server_name: slurmserver
	# IP address of the SLURM server
	slurm_server_ip: 127.0.0.1
	# Prefix to set to the SLURM working nodes
	slurm_vnode_prefix: vnode-
	# List of IPs of the WNs
	slurm_wn_ips: []
	# List of the name of the WNs
	slurm_wn_nodenames: []
	# Number of CPUs of the WNs
	slurm_wn_cpus: 1

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
