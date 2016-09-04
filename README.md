# ansible-multienv-base
Template for ansible project that allows to easily manage multiple environments (e.g. staging, test, production) and harden new hosts

# Description
This is my base template for ansible projects requiring multiple environment deployment. 
General idea is to use multiple inventories, and be able to easily add new hosts / groups / environments, each with own variables.

# Structure
```
|- hosts
|  |- shared-secrets.yml  # encrypted vars, used in all environments
|  |- shared-vars.yml     # not encrypted vars, used in all environments
|  |- environment1        # directory for environment (e.g. test or staging)
|  |  |- inventory        # inventory file with definitions of all required hosts
|  |  |- secrets.yml      # encrpted vars for this environment
|  |  |- groups_vars
|  |     |- all.yml       # normal group_vars, like in typical ansible project
|  |- environment2        # directory for other environment
|     |- inventory
|     |- secrets.yml
|     |- groups_vars
|        |- all.yml
|- roles                  # all ansible roles used in playbooks
|  |- role1
|     |- ...
|- playbook1.yml          # here we put our playbooks
```

# Usage
```
git clone https://github.com/Valian/ansible-multienv-base
```
Next, we must create our playbooks, fill inventories and variables, create roles etc. 
To encrypt secret variables file, use
```
ansible-vault encrypt path-to-file     # Caution! Pass must be the same in every encrypted file
```

There is also a special task, that we should include in pre_tasks in each playbook, called tasks/load_vars.yml.
To run playbook against specified environemnt, we must use
```
ansible-playbook -i hosts/desired_env/inventory --ask-vault-pass playbook.yml
```

Hope this will be helpful to somebody.

You can find me on Twitter @jskalc
