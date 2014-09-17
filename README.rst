Ansible modules for Proxmox
============================

Ansible modules to manage Proxmox

proxmoxer_ct
------------

Manage [Proxmox](https://www.proxmox.com) containers via the proxmox api using the [proxmoxer](https://pypi.python.org/pypi/proxmoxer) library.

Can create, stop, start, restart and create containers.


Example
^^^^^^^

In this example we will create a Ubuntu 14.04 container on the proxmox host 10.0.0.1.

The group_vars/all file has the following contents.
::

        proxmox:
            username: root
            host: 10.0.0.1

The test.yml file has the following contents.
::
    
        ---

        - hosts: 127.0.0.1
          connection: local                                  # Run this task on the local host.
          vars_prompt:
            - name: "proxmox_password"                       # Prompt the user for the Proxmox password.
              prompt: "Enter Proxmox Password"
              private: yes

          tasks:
            - name: Run module proxmoxer_ct
              proxmoxer_ct: state={{ ct_state }} 
                            hostname={{ proxmox.host }}      # Hostname or IP of proxmox server.
                            username={{ proxmox.username }}  # Proxmox User.
                            password={{ proxmox_password }}  # Proxmox User Password.
                            ct_id=999                        # The container ID.
                            ct_ostemplate='ubuntu 14.04'     # Do some template guessing.

Running the playbook to create a new container.
::

        (ve)~/P/ansible ❯❯❯ ansible-playbook -i 127.0.0.1, test.yml -e ct_state=present
        Enter Proxmox Password: 

        PLAY [127.0.0.1] ************************************************************** 

        GATHERING FACTS *************************************************************** 
        ok: [127.0.0.1]

        TASK: [Run module proxmoxer_ct] *********************************************** 
        ok: [127.0.0.1]

        PLAY RECAP ******************************************************************** 
        127.0.0.1                  : ok=2    changed=0    unreachable=0    failed=0   


