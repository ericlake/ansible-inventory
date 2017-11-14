# Ansible Inventory

Purpose
-------

This repository stores all the inventories for the DREAM  ansible plays.


Structure
---------

The structure of the repository is similar to the following:

```
.
├── badger
│   ├── prod
│   │   ├── artifacts
│   │   ├── cephadmin
│   │   ├── director
│   │   └── postgress
│   └── stag
│       ├── artifacts
│       ├── cephadmin
│       ├── director
│       └── postgress
├── clan
│   └── prod
│       └── artifacts
└── README.md

```

You'll notice that the inventory files are separated by network, tier, and role, and inside of each inventory file, the parent-child relationship of groups is spelled out clear using INI syntax:

```
[badger:children]
artifacts

[artifacts]
artifacts-01.prod.badger.net

```

This allows for the plays to use all the host and group vars associated with both the parent and child groups, but also creates some seperation so that a single Ansible command is not mistakenly run across multiple networks, environment tiers, or roles. Also of note, is the `net_name` variable that is defined as a group_var for the network group (i.e. badger, clan, etc). This variable is used by the playbooks to execute network-specific tasks on hosts, if required. As a result, you should ensure that the role-based group is a child of the network group that contains `net_name` in it's group_vars.


Usage
-----

Inventory files can be called like so:

```
ansible-playbook -i inventory/badger/prod/artifacts example_playbook.yml
```

