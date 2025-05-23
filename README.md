
# Ansible

This repository is a summary of Ansible, check the official documentation on the links below.

Web site: https://www.ansible.com/<br>
Documentation: https://docs.ansible.com/<br>
GitHub: https://github.com/ansible/ansible<br>

# Summary

1. [Introduction](#introduction)<br>
2. [Ansible concepts](#ansible_concepts)<br>
3. [How to install on Ubuntu 24](#install_ubuntu24)<br>
3.1. [Creating configuration file](#creating_conf_file)<br>
3.2. [Creating inventory (YAML)](#creating_inventory)<br>
4. [Commands](#commands)<br>
5. [Inventory](#inventory)<br>
5.1. [Adding Hosts](#add_hosts)<br>
5.2. [Adding Groups](#add_groups)<br>
5.3. [Dynamic Inventory](#dynamic_inventory)<br>
6. [Introducing Playbooks](#intro_playbooks)<br>
6.1. [Runnig playbook locally (localhost)](#run_playbooks)<br>
7. [Variables](#variables)<br>
7.1. [Variable types](#variables_types)<br>
7.2. [Variables in the Playbook file (.yml)](#variables_in_playbooks)<br>
7.3. [External Variables file (.yml)](#external_variables_file)<br>
7.4. [Inventory Variables](#inventory_variables)<br>
7.5. [group_vars & host_vars](#group_host_vars)<br>
7.6. [Special Variables](#special_variables)<br>
7.7. [Ansible Facts](#ansible_facts)<br>
7.8. [Registering variables](#registering_variables)<br>
7.9. [Variables precedence](#variables_precedence)<br>
8. [Playbooks](#playbooks)<br>
8.1. [Keywords](#playbooks_keywords)<br>
8.2. [Tasks](#playbooks_tasks)<br>
8.2.1. [Conditionals (when)](#playbooks_conditionals)<br>
8.2.2. [Loops](#loops)<br>
8.2.2.1. [Loop](#loop)<br>
8.2.2.2. [Loop control](#loop_control)<br>
8.3. [Blocks](#blocks)<br>
8.3.1. [Handling tasks failures with `rescue`](#blocks_rescue)<br>
8.3.2. [`always` section](#blocks_always)<br>
8.4. [Handlers](#handlers)<br>
8.5. [Importing a playbook](#importing_playbooks)<br>
8.6. [Importing tasks from another file](#importing_tasks)<br>
8.7. [Tags](#tags)<br>
9. [Modules](#modules)<br>
9.1. [Executing modules from the command line](#running_modules_from_command_line)<br>
9.2. [Executing modules from playbooks](#running_modules_from_playbooks)<br>
10. [Templates](#templates)<br>
10.1. [Variable Expressions](#templates_variable_expressions)<br>
10.2. [Conditionals](#templates_conditionals)<br>
10.3. [Loops](#templates_loops)<br>
11. [Plugins](#plugins)<br>
11.1. [Filter](#filter)<br>
12. [Roles](#roles)<br>
12.1. [Creating a role (task)](#creating_roles)<br>
12.2. [Using roles](#using_roles)<br>
12.2.1. [Play level](#roles_play_level)<br>
12.2.2. [Task level](#roles_task_level)<br>
12.2.3. [Running specific task files](#roles_specific_task)<br>
13. [Vault](#vault)<br>
13.1. [Vault Password](#vault_password)<br>
13.2. [Variable-level encryption](#vault_variable_level)<br>
13.2.1. [Encrypting variables](#vault_variable_encryption)<br>
13.2.2. [Viewing encrypted variables](#view_encryt_variable)<br>
13.3. [File-level encryption](#vault_file_level)<br>
13.3.1. [Encrypting files](#vault_encrypting_files)<br>
13.3.2. [Decrypting files](#vault_decrypting_files)<br>
13.3.3. [Rotating password](#vault_rotating_password)<br>
13.4. [Vault ID - Multiple passwords](#vault_id)<br>
14. [Collection](#collection)<br>
15. [Developing Modules](#developing_modules)<br>
15.1. [Verifying your module locally](#verify_your_module)<br>
15.1.1. [Using Ansible adhoc command](#verify_your_module_adhoc_command)<br>
15.1.2. [Using the module in a Playbook](#verify_your_module_in_a_playbook)<br>
15.1.3. [Using Python](#verify_your_module_using_python)<br>
16. [Developing Collections](#developing_collections)<br>
17. [Developing Plugins](#developing_plugins)<br>
18. [Env Vars](#env_vars)<br>
19. [Troubleshooting](#troubleshooting)<br>

# 1. Introduction <a name="introduction"></a>

Ansible is an open source IT automation engine that automates provisioning, configuration management, application deployment, orchestration, and many other IT processes.

In brief, Ansible connects to remote hosts via SSH to execute commands and Python scripts previously sent by SCP.

# 2. Ansible concepts <a name="ansible_concepts"></a>

- **Control node (controller)** The machine from which you run the Ansible Commands. Ansible needs to be installed only on this machine.</br>
- **Managed nodes (hosts)** Target devices you aim to manage with Ansible.</br>
- **Inventory** List of hosts and groups.</br>
- **Playbook** A collection of plays. A file coded in YAML.</br>
  - **Play** Run tasks on a host or a collection of hosts.</br>
  - **Taks** Call functions defined as Ansible Modules (coded in Python)</br>
  - **Roles** A reusable Ansible content (tasks, variables, ...) for user inside a Play.</br>
  - **Handlers** Handlers are tasks that only run when notified (when the task returns a ‘changed=True’ status).</br>
- **Variables** Variables store and retrieve values that can be referenced in playbooks, roles, templates and other Ansible components.</br>
- **Templates** Templates allow you to create new files on the nodes using predefined models based on the Jinja2 templating system.</br>
- **Vault** Ansible Vault is a feature of ansible that allows you to keep sensitive data such as passwords or keys in encrypted files.</br>
- **Modules** Usually a Python script sent to each host (when needed) to accomplish the action in each Task.</br>
- **Plugins** Expands the Ansible's core capactibilities.</br>
  - **Action Plugins** let you integrate local processing and local data with module functionality</br>
  - **Cache Plugins** store gathered facts and data retrieved by inventory plugins.</br>
  - **Callback Plugins** add new behaviors to Ansible when responding to events.</br>
  - **Connection Plugins** allow Ansible to connect to target hosts so it can execute tasks on them.</br>
  - **Filter Plugins** manipulate data.</br>
  - **Inventory Plugins** parse inventory sources and form an in-memory representation of the inventory.</br>
  - **Lookup Plugins** pull in data from external data stores. </br>
  - **Test Plugins** verify data.</br>
  - **Vars Plugins** inject additional variable data into Ansible runs that did not come from an inventory source, playbook, or command line.</br>
- **Collections** A format in which Ansible content is distributed that can contain playbooks, roles, modules, and plugins. You can install and use collections through [Ansible Galaxy](https://galaxy.ansible.com/ui/).</br>

# 3 How to install on Ubuntu 24 <a name="install_ubuntu24"></a>

**Installing in isolated environments with pipx (Recommended)**

```shell
sudo apt update
sudo apt install pipx -y
pipx install --include-deps ansible
pipx ensurepath
source ~/.bashrc

# Updating
pipx upgrade --include-injected ansible

# Installing aditional python modules
pipx runpip ansible -- install <module>
```

**Installing with pip3**

```shell
sudo apt update
sudo apt install python3-pip -y

# Choose only one of the two lines below, choosing Global or Local installation
sudo pip3 install ansible           # Global
pip3 install --user ansible         # Local

echo 'export PATH="$PATH:$HOME/.local/bin"' >> ~/.bashrc
source ~/.bashrc
```

## 3.1 Creating configuration file <a name="creating_conf_file"></a>

```shell
sudo mkdir -p /etc/ansible
sudo chown $USER:$USER /etc/ansible
ansible-config init --disabled -t all > /etc/ansible/ansible.cfg
```

## 3.2 Creating inventory (YAML) <a name="creating_inventory"></a>

```shell
touch /etc/ansible/hosts
```

Example for localhost and one remote host called device1.

```yaml
all:
  hosts:
    localhost:
      ansible_connection: local
    device1:
      ansible_host: "192.168.0.10"
      ansible_ssh_user: "leo"
      ansible_ssh_private_key_file: "/home/leo/.ssh/id_ed25519"
  vars:
    ansible_python_interpreter: "/usr/bin/python3"
```

# 4 Commands <a name="commands"></a>

**Version**

```shell
ansible --version
```

**Show inventory**

```shell
ansible-inventory --list
ansible-inventory --graph
ansible-inventory --graph <group_name>
ansible-inventory --host <host>
```

**Run Playbook**

```shell
# General
ansible-playbook myplaybook.yml                          # run the playbook for all hosts defined inside the playbook
ansible-playbook p1.yml p2.yml                           # running multiple playbooks
ansible-playbook myplaybook.yml -l localhost,device1     # (--limit) limit selected hosts (comma separated)
ansible-playbook myplaybook.yml -i /tmp/inventory.yml    # (--inventory) specify the inventory (comma separated)
ansible-playbook myplaybook.yml -vvv                     # verbose mode (-v, -vvv, -vvvv)
ansible-playbook myplaybook.yml --check                  # Check mode is just a simulation

# Extra variables at runtime (variables defined in the command line with -e take precedence over other variable definitions and remain immutable throughout the playbook execution)
ansible-playbook myplaybook.yml -e username=leo                   # (--extra_vars) set additional variables as key=value or YAML/JSON
ansible-playbook myplaybook.yml -e "username=leo password=*****"  # Multiples key=value
ansible-playbook myplaybook.yml -e '{"username":"leo"}'           # inline JSON
ansible-playbook myplaybook.yml -e @/var/external_vars.yml        # if filename prepend with @

# Tags and Tasks
ansible-playbook myplaybook.yml --list-tags              # list all available tags
ansible-playbook myplaybook.yml --list-tasks             # list all tasks that would be executed
ansible-playbook myplaybook.yml --tags                   # (-t) only run plays and tasks tagged with these values
ansible-playbook myplaybook.yml --skip-tags              # only run plays and tasks whose tags do not match these values

# Vault pass
ansible-playbook myplaybook.yml --ask-vault-pass         # ask for vault password
ansible-playbook myplaybook.yml --vault-password-file pass_file    # vault password file
```

**Doc**
```shell
ansible-doc shell                                         # shows documentation of the shell module
ansible-doc -t callback -l                                # shows callback plugins
```

**Run Module**

```shell
ansible localhost -m ping                                 # connection test            
ansible localhost -m setup --tree facts.d/                # write facts to file
ansible webservers -m command -a "/sbin/reboot -t now"
ansible webservers -m service -a "name=httpd state=started"
```

**Installing collections with Ansible Galaxy**

```shell
ansible-galaxy collection init my_namespace.my_collection             # Create collection with a template
ansible-galaxy collection build path/to/my_namespace/my_collection    # Build collection
ansible-galaxy collection install path/to/my_namespace/my_collection  # Install collection
ansible-galaxy collection install path/to/my_namespace-my_collection-1.0.0.tar.gz # Install builded collection

ansible-galaxy collection install ansible.utils           # Install the collection ansible.utils
ansible-galaxy collection list                            # List installed collections
```

**Vault**

```shell
ansible-vault create myfile.yml                     # Create new vault encrypted file
ansible-vault decrypt myfile.yml                    # Decrypt vault encrypted file
ansible-vault edit myfile.yml                       # Edit vault encrypted file
ansible-vault view myfile.yml                       # View vault encrypted file
ansible-vault encrypt myfile.yml                    # Encrypt YAML file
ansible-vault encrypt_string 'value' --name 'key'   # Encrypt a string
ansible-vault rekey myfile.yml                      # Re-key a vault encrypted file

# Arguments
--vault-password-file
--ask-vault-pass
--vault-id
```

# 5 Inventory <a name="inventory"></a>

[[doc]](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)

The default location for this file is `/etc/ansible/hosts`. You can specify a different inventory file at the command line using the -i \<path\> option or in the ansible.cfg file updating the entry `inventory=`.

The most common inventory file formats are INI and YAML (preffered).

Ansible has two special groups, `all` and `ungrouped`. The `all` group contains every host. The `ungrouped` group contains all hosts that don't have another group aside from `all`.

# 5.1 Adding Hosts <a name="add_hosts"></a>

In the inventory file add the host name to a group in the `hosts:` session, eding with `:`.

```yaml
all:
  hosts:
    device1:    # host name
    device2:    # host name
```

# 5.2 Adding Groups <a name="add_groups"></a>

In the inventory file add the group name ending with `:`.

```yaml
my_group1:      # group name
  hosts:        # hosts of the group
```

Add to this group the session `hosts:` to specify hosts and the session `children:` to specify other groups as child of this group.

```yaml
my_group3:      # group name
  hosts:        # hosts of the group
  children:     # groups of the group
    my_group1:
    my_group2:
```
## 5.3 Dynamic Inventory <a name="dynamic_inventory"></a>

The inventory can be defined in runtime, in a task with the module [add_host](https://github.com/leofds/ansible-essentials/blob/master/builtin_modules.md#add_host)

# 6 Introducing Playbooks <a name="intro_playbooks"></a>

### Execution

- A playbook runs in order from top to bottom, by default, each taks one at a time, against all machines of hosts in parallel. After executed on all target machines, Ansible moves on to the next task. If a task fail on a host, Ansible takes that host out of the rotation for the rest of the playbook.

**Sample Playbook file (myplaybook.yml)**

```yaml
- name: Sample Playbook
  hosts: all
  gather_facts: false     # Disable facts (optional)

  tasks:
    - name: Display a debug message
      debug:
        msg: "Hello Admin"
```
Running the playbook on device1 (if device1 is in the inventory)

```shell
ansible-playbook myplaybook.yml -l device1
```

**output**
```

PLAY [Sample PLaybook] ***********************************************************************************************************************************************

TASK [Display a debug message] ***************************************************************************************************************************************
ok: [device1] => {
    "msg": "Welcome Admin"
}

PLAY RECAP ***********************************************************************************************************************************************************
device1                    : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## 6.1 Runnig playbook locally (localhost) <a name="run_playbooks"></a>

Set the connection plugin to `local` (prefered) or exchange the SSH key locally. (`cat "${HOME}/.ssh/id_ed25519.pub" >> "${HOME}/.ssh/authorized_keys"`)

# 7 Variables <a name="variables"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html)

Ansible uses `Jinja2` to access variables dynamically using `{{ variable_name }}`.

```yaml
...
- name: Display a debug message
  debug:
    msg: "Hello {{ username }}"
```

## 7.1 Variable types <a name="variables_types"></a>

**Simple variable**

```yaml
base_path: '/etc/config'
```
Referencing a simple variable
```yaml
app_path: "{{ base_path }}"
```

**Multiple lines string**

```yaml
message: >
  Your long
  string here.
```

```yaml
message: |
  this is a very
  long string
```

- `>`, `|`: "clip" keep the line feed, remove the trailing blank lines.
- `>-`, `|-`: "strip": remove the line feed, remove the trailing blank lines.
- `>+`, `|+`: "keep": keep the line feed, keep trailing blank lines.

**List**

```yaml
region:
- northeast
- southeast
- midwest
```
Referencing list variables
```yaml
region: "{{ region[0] }}"
```

**Dictionary**

```yaml
foo:
  field1: one
  field2: two
```
Referencing key:value dictionary variables
```yaml
field: "{{ foo['field1'] }}"
field: "{{ foo.field1 }}"
```

## 7.2 Variables in the Playbook file (.yml) <a name="variables_in_playbooks"></a>

```yml
- name: Sample Playbook
  hosts: all
  vars:
    username: 'leo'
    password: '*****'
  vars_files:
    - /vars/external_vars.yml

  tasks:
    - name: Display a debug message
      vars:
        username: 'leo'
```

## 7.3 External Variables file (.yml) <a name="external_variables_file"></a>

```yml
username: 'leo'
password: '*****'
```

## 7.4 Inventory Variables <a name="inventory_variables"></a>

```yaml
mygroup:
  hosts:
    device1:
      username: "leo"       # host variable
  vars:
    dummy: "superserver"    # group variable
    vars_file: group_vars/webservers_vars.yml
```

## 7.5 group_vars & host_vars <a name="group_host_vars"></a>

group_vars/host_vars variables files are automatically loaded when running a playbook. 

**group_vars**

Create the dir `/etc/ansible/group_vars`. 

```shell
/etc/ansible/group_vars/mygroup   # can optionally end in '.yml', '.yaml', or '.json'
```

**host_vars**

Create the dir `/etc/ansible/host_vars`. 

```shell
/etc/ansible/host_vars/localhost   # Variables file of localhost
```

**Multiple files for a host/group**

Create directories named instead of a file. Ansible will read all the files in these directories.

```yaml
/etc/ansible/group_vars/mygroup/network_settings
/etc/ansible/group_vars/mygroup/cluster_settings
```

## 7.6 Special Variables <a name="special_variables"></a>

[[doc]](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html)

```yaml
ansible_host: "192.168.0.10"
ansible_ssh_user: "leo"
ansible_ssh_pass: "******"
ansible_ssh_private_key_file: "/home/leo/.ssh/id_ed25519"
ansible_python_interpreter: "/usr/bin/python3"
```

```yaml
ansible_verbosity
```

## 7.7 Ansible Facts <a name="ansible_facts"></a>

Ansible facts are data related to your remote systems.
By default, Ansible gathers facts at the beginning of each play.

```yaml
- name: Print all available facts
  debug:
    var: ansible_facts
```

Disabling facts

```yaml
- name: Sample Playbook
  hosts: all
  gather_facts: false     # Disable facts (optional)
```

## 7.8 Registering variables <a name="registering_variables"></a>

[[doc]](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html)

A variable can be created from the Task output with the keyword `register`.

```yaml
- hosts: all
  tasks:
    - name: Runs a shell command registering the output to a variable
      shell: whoami
      register: result

    - name: Reads the variable
      debug:
        msg: "Output: {{ result.stdout }}, return code: {{ result.rc }}"
```

## 7.9 Variables precedence <a name="variables_precedence"></a>

The order of precedence from least to greatest (the last listed variables override all other variables):

1. command line values (for example, -u my_user, these are not variables)
2. role defaults (as defined in Role directory structure) 1
3. inventory file or script group vars 2
4. inventory group_vars/all 3
5. playbook group_vars/all 3
6. inventory group_vars/* 3
7. playbook group_vars/* 3
8. inventory file or script host vars 2
9. inventory host_vars/* 3
10. playbook host_vars/* 3
11. host facts / cached set_facts 4
12. play vars
13. play vars_prompt
14. play vars_files
15. role vars (as defined in Role directory structure)
16. block vars (only for tasks in block)
17. task vars (only for the task)
18. include_vars
19. set_facts / registered vars
20. role (and include_role) params
21. include params
22. extra vars (for example, -e "user=my_user")(always win precedence)

# 8 Playbooks <a name="playbooks"></a>

[[Error handling in playbooks]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_error_handling.html#aborting-on-the-first-error-any-errors-fatal)

## 8.1 Keywords <a name="playbooks_keywords"></a>

[[doc]](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html)

- Play
- Role
- Block
- Task

### Play

```yaml
- name: Sample                         # Identifier. Can be used for documentation, or in tasks/handlers.

  hosts: <pattern>                     # Common patterns (they can be combined)
                                       #   all - All hosts
                                       #   host1 - One host
                                       #   host1:host2 (host1,host2) - Multiple hosts/groups
                                       #   all:!host1 - All hosts/groups except one
                                       #   group1:&group2 - Any hosts in the group1 that are also in the group2

  gather_facts: false                  # Disable facts to improve performance (default true)

  connection: <plugin>                 # Change the connection plugin. Lists available plugins with `ansible-doc -t connection -l`.
                                       # ssh - connect via ssh client binary (default)
                                       # local - execute on controller

  collections:                         # Using a collection.
    - my_namespace.my_collection

  strategy: free                       # Strategy of task execution, linear or free

  become: yes                          # Ansible allows you to ‘become’ another user, different from the user that logged into the machine (remote user).
                                       # This is done using existing privilege escalation tools such as sudo, su, pfexec, doas, pbrun, dzdo, ksu, runas, machinectl and others.
  become_method: su
  become_user: nobody                  # default root
  become_pass:
  become_flags: "-i"                   # loads the user's login shell as if the user logged in interactively. It loads the environment variables from the user's profile, such as /etc/profile, ~/.bash_profile, or ~/.bashrc, depending on the shell configuration. Ansible uses non-interactive sessions.
  become_flags: '-s /bin/sh'
  
  service:                             # Controls services on remote hosts. Ensure the httpd service is running
    name: httpd
    state: started

  timeout:                             # Time limit for the task to execute in, if exceeded Ansible will interrupt and fail the task.

  vars:                                # Dictionary/map of variables
    username: 'leo'
  vars_files:                          # List of files that contain vars to include in the play.
    - /vars/external_vars.yml
  vars_prompt:                         # list of variables to prompt for
    - name: username                   # variable name
        prompt: What is your username? # prompt message
        private: false                 # makes the user input visible (hidden by default)
        default: "1.0"                 # default value
        confirm: true                  # request confirmation
        encrypt: sha512_crypt          # encript. (use private = true)
        unsafe: true                   # allow special chars
        salt_size: 7

  check_mode: <boolean>                # If you want certain tasks to run in check mode always, or never.
                                       # true - this task will never make changes to the system
                                       # false - this task will always make changes to the system

  any_errors_fatal:  <boolean>         # If you set any_errors_fatal and a task returns an error, Ansible finishes the fatal task on all hosts in the current batch and then stops executing the play on all hosts. Subsequent tasks and plays are not executed. You can recover from fatal errors by adding a rescue section to the block. You can set any_errors_fatal at the play or block level.

  ignore_errors: <boolean>             # Ignore task failures and continue with play. It does not affect connection errors.
                                       # By default, Ansible stops executing tasks on a host when a task fails on that host. You can use ignore_errors to continue despite of the failure. The ignore_errors directive only works when the task can run and returns a value of ‘failed’. It does not make Ansible ignore undefined variable errors, connection failures, execution issues (for example, missing packages), or syntax errors.
  ignore_unreachable: <boolean>        # Ignore task failures due to an unreachable host and continue with the play. This does not affect other task errors

  max_fail_percentage: <number>        # Can be used to abort the run after a given percentage of hosts in the current batch has failed. This only works on linear or linear-derived strategies.

  debugger: <options>                  # Enable debugging tasks based on the state of the task result.
                                       # Options: always, never, on_failed, on_unreachable, on_skipped

  diff: <boolean>                      # Toggle to make tasks return ‘diff’ information or not

  no_log: <boolean>                    # Controls information disclosure.

  order: <options>                     # Controls the sorting of hosts as they are used for executing the play.
                                       # options: inventory (default), sorted, reverse_sorted, reverse_inventory and shuffle

  tags                                 # Tags applied to the task or included tasks

  throttle: <number>                   # Limit the number of concurrent task runs on task, block and playbook level. This is independent of the forks and serial settings, but cannot be set higher than those limits. For example, if forks is set to 10 and the throttle is set to 15, at most 10 hosts will be operated on in parallel.

  environment
  fact_path
  force_handlers
  gather_facts
  gather_subset
  gather_timeout
  module_defaults
  port
  remote_user
  run_once
  serial
  strategy
  timeout

  tasks
  post_tasks
  pre_tasks
  handlers
  roles
```

### Tasks

```yaml
  tasks:
    delegate_to: localhost
    run_once: true
    notify:                    # List of handlers to notify when the task returns a ‘changed=True’ status.
    ignore_errors:             # Boolean to ignore the task failures and continue with the play.
    failed_when:               # Conditional expression that overrides the task 'failed' status.
    changed_when:              # with true: the task is always reported as changed
```

**Block**

```yaml

```

**Role**

```yaml

```

## 8.2 Tasks <a name="playbooks_tasks"></a>

### 8.2.1 Conditionals (when) <a name="playbooks_conditionals"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_conditionals.html)

Similar to `if`. The code below skipes the task.

```yaml
- name: Sample
  hosts: localhost
  vars:
    value: false

  tasks:
    - name: Test
      debug:
        msg: "Value is true"
      when: value
```

**Logic operators**

```yaml
when: status == "enabled"
when: status != "enabled"
when: status > 5
when: contents.stdout == ""
when: version | int >= 4
when: temperature | float > 90
when: epic or monumental | bool
when: not epic
when: contents.stdout.find('hi') != -1
when: contents.rc == 127
when: result is failed
when: result is succeeded
when: result is skipped
when: result is changed
when: foo is defined
when: bar is undefined
when: x is not defined
when: my_string.startswith('prefix_')
when: "'substring' in my_string"
when: "'servers' in group_names"
```

**Boolean operators AND/OR**

```yaml
when:
  ansible_facts['distribution'] == "Ubuntu" and ansible_facts['distribution_major_version'] == "24"

when:
  - ansible_facts['distribution'] == "Ubuntu"
  - ansible_facts['distribution_major_version'] == "24"
```

```yaml
when: value == "10" or value == "5"
```

```yaml
when: (name == "leo" and version == "5") or
      (name == "admin" and version == "6")
```

### 8.2.2 Loops <a name="loops"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html)

### 8.2.2.1 Loop <a name="loop"></a>

**Simple list**

The `loop` will run the task once per list item.

```yaml
  tasks:
    - name: Test
      debug:
        msg: "Fruit is {{ item }}"
      loop:
        - banana
        - apple
        - orange
```

`with_list` keyword is equivalent to `loop`.

```yaml
      with_list:
        - banana
        - apple
        - orange
```

**Variables**

```yaml
loop: "{{ list_of_fruits }}"
loop: "{{ ['banana', 'apple', 'orange'] }}"
```

**Subkeys**

```yaml
  tasks:
    - name: Test
      debug:
        msg: "User {{ item.name }}"
      loop:
        - { name: 'user1', group: 'root' }
        - { name: 'user2', group: 'local' }
```

**Dictionary**

```yaml
  tasks:
    - name: Test
      vars:
        user_data:
          name: 'leo'
          group: 'root'
      debug:
        msg: "User {{ item.key }} = {{ item.value }}"
      loop: "{{ user_data | dict2items }}"
```

#### 8.2.2.2 Loop control <a name="loop_control"></a>

**Until condition**

```yaml
  tasks:
    - name: Test
      shell: /usr/bin/foo
      register: result
      until: result.stdout.find("all systems go") != -1
      retries: 3
      delay: 1
```

## 8.3 Blocks <a name="blocks"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_blocks.html)

A block is a group of tasks. All taks in the block inherit the block directives.

```yaml
  tasks:
    - name: Install, Setup, Start
      block:
        - name: Install
          # ...
        - name: Setup
          # ...
        - name: Start
          # ...
      when: ansible_facts['distribution'] == 'Ubuntu'
```

### 8.3.1 Handling tasks failures with `rescue` <a name="blocks_rescue"></a>

Similar to exception handling in many programming languages, `rescue` block specify tasks to run when a task in the block fails.

```yaml
  tasks:
    - name: Handle the error
      block:
        - name: Some command
          # ...
      rescue:
        - name: Print errors
          debug:
            msg: 'Error'
```

### 8.3.2 `always` section <a name="blocks_always"></a>

No matter what the task status in the block is, the tasks in the sessions `always` are always executed after the block tasks.

```yaml
  tasks:
    - name: Always do
      block:
        - name: Some command
          # ...
      always:
        - name: Always do this
          debug:
            msg: 'This always executes'
```

## 8.4 Handlers <a name="handlers"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html)

Handlers are tasks that only run when notified. Usually when a task made a change in a machine.

```yaml
  tasks:
    - name: Install service
      # ...
      notify:
        - Restart service

  handlers:
    - name: Restart service
      # ...
```

## 8.5 Importing a playbook <a name="importing_playbooks"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse.html)
[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_playbook_module.html)

```yaml
- ansible.builtin.import_playbook: myplaybook1.yml
- ansible.builtin.import_playbook: myplaybook1.yml

- name: Include a play after/before another play
  ansible.builtin.import_playbook: otherplays.yml
  when: not my_file.stat.exists
  vars:
    value: 'dummy'
```

## 8.6 Importing tasks from another file <a name="importing_tasks"></a>

[[doc]](https://docs.ansible.com/ansible/2.8/modules/import_tasks_module.html)

```yaml
- name: Run tasks from another file
  hosts: localhost
  tasks:
    - import_tasks: tasks_file.yml
```

Sample file `tasks_file.yml`

```yaml
- name: Sample task 1
  debug:
    msg: "Hello 1"

- name: Sample task 2
  debug:
    msg: "Hello 2"
```

## 8.7 Tags <a name="tags"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tags.html)

Add tags to your tasks to skip or run tasks individually.

```yaml
- name:
  debug:
    msg: "Tag 1"
  tags: tag1

- name:
  debug:
    msg: "Tag 2"
  tags:
    - tag2
```

Running the playbook

```yaml
ansible-playbook myplaybook.yml --tags tag1,tag2         # (-t) only run plays and tasks tagged with these values
ansible-playbook myplaybook.yml --skip-tags tag1         # only run plays and tasks whose tags do not match these values
```

# 9 Modules <a name="modules"></a>

[[doc]](https://docs.ansible.com/ansible/latest/module_plugin_guide/index.html)

List of common [builtin modules](https://github.com/leofds/notes/tree/master/ansible/builtin_modules.md)

Modules (also referred to as “task plugins” or “library plugins”) are discrete units of code that can be used from the command line or in a playbook task. A module is a script that Ansible runs locally or remotely, and collects return values.

All modules return JSON format data [[doc]](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html). This means modules can be written in any programming language, but Python is a common choice.

> **_NOTE:_** Modules should be idempotent, and should avoid making any changes if they detect that the current state matches the desired final state.

## 9.1 Executing modules from the command line <a name="running_modules_from_command_line"></a>

```yaml
ansible device1 -m ping
ansible device1 -m command -a "/sbin/reboot -t now"        # With argument
ansible device1 -m service -a "name=httpd state=started"   # With arguments key=value
```

## 9.2 Executing modules from playbooks <a name="running_modules_from_playbooks"></a>

```yaml
- name: reboot the servers            # Task name
  command: /sbin/reboot -t now        # Module name and arguments
```

```yaml
- name: restart webserver             # Task name
  service:                            # Module name
    name: httpd                       # Module args
    state: restarted                  # Module args
```

# 10 Templates <a name="templates"></a>

[doc](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_templating.html) [template_module](https://docs.ansible.com/ansible/2.8/modules/template_module.html)

In Ansible, templates files are files written using Jinja2 templating language that allow dynamic content to be generated during playbook execution. They are commonly used to configure services, such as configuration files for Nginx, Apache, systemd, and others.

- File extension is usually .j2 (e.g., nginx.conf.j2).
- Used with the template module in Ansible.
- Allow you to insert variables, conditionals, loops, and more into files that will be rendered and copied to target machines.

**Example**

`foo.conf.j2`

```
server {
  name {{ server_name }};
}
```

```yaml
- hosts: all
  vars:
    server_name: "example"

  tasks:
    - name: Template a file to /etc/files.conf
      template:
        src: /mytemplates/foo.conf.j2
        dest: /etc/foo.conf
```

In Ansible templates, you can use various types of statements to control logic and flow in the template. These include variable expressions, control statements like conditionals and loops, and filters:

## 10.1 Variable Expressions <a name="templates_variable_expressions"></a>

Used to insert variable values into the file.

```jinja2
Hello, my name is {{ name }}
```

## 10.2 Conditionals (if, elif, else) <a name="templates_conditionals"></a>

Allow logic based on variable values.

```jinja2
{% if env == "production" %}
ENV=production
{% elif env == "staging" %}
ENV=staging
{% else %}
ENV=development
{% endif %}
```

## 10.3 Loops (for) <a name="templates_loops"></a>

Used to iterate over a list or dictionary.

```jinja2
{% for user in users %}
- name: {{ user.name }}
  uid: {{ user.uid }}
{% endfor %}
```

# 11 Plugins <a name="plugins"></a>

[[ansible.builtin]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)

Plugins are pieces of code that augment Ansible’s core functionality. This section covers the various types of plugins that are included with Ansible:

# 11.1 Filter <a name="filter"></a>

[[filter-plugins]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html#filter-plugins)
[[playbook-filters]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_filters.html)

**Default values**

```yaml
{{ some_variable | default(5) }}
```

**Optional Variable**

```yaml
mode: "{{ item.mode | default(omit) }}"    # mode does not send a value for mode if the item.mode is not defined
```

**Mandatory values**

```yaml
{{ variable | mandatory }}
```

**Ternary**

```yaml
{{ condition | ternary(true_value, false_value) }}
{{ condition | ternary(true_value, false_value, omit) }}  # Third value on null
```

```yaml
myvar: "value={{ 'value_if_true' if some_condition else 'value_if_false' }}"
```

**Base64**

```yaml
{{ 'bG9sYQ==' | b64decode }}         # Decode a Base64 string
{{ 'Jose'| b64encode }}              # Encode a string as Base64
```

**Path basename**

```yaml
{{ mypath | basename }}              # Get the last name of a file path, like 'foo.txt' out of '/etc/asdf/foo.txt'
```

**Cast**

```yaml
{{ (a == b) | bool }}                # Cast into a boolean
```

**Checksum**

```yaml
{{ 'test2' | checksum }}             # Checksum (SHA-1) of input data. => "109f4b3c50d7b0df729d299bc6f8e9ef9066971f"
```

**Combination**

```yaml
{{ [1,2,3,4,5] | combinations(2) }}  # Combinations from the elements of a list. => [ [ 1, 2 ], [ 1, 3 ], [ 1, 4 ], [ 1, 5 ], [ 2, 3 ], [ 2, 4 ], [ 2, 5 ], [ 3, 4 ], [ 3, 5 ], [ 4, 5 ] ]
{{ {'a':1, 'b':2} | ansible.builtin.combine({'b':3, 'c':4}) }}  # Combine two dictionaries. => {'a':1, 'b':3, 'c': 4}
```

**Comment** [ansible.builtin.comment filter – comment out a string](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/comment_filter.html#ansible-collections-ansible-builtin-comment-filter)


# 12 Roles <a name="roles"></a>

[[doc]](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

Roles are a reusable Ansible content (tasks, variables, ...) for user inside a Play.

An Ansible role has a defined directory structure with seven main standard directories. You must include at least one of these directories in each role. You can omit any directories the role does not use.

```
roles/
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies
        library/          # roles can also include custom modules
        module_utils/     # roles can also include custom module_utils
        lookup_plugins/   # or other types of plugins, like lookup in this case
```

Default locations:
- in collections, if you are using them
- in a directory called roles/, relative to the playbook file
- in the configured roles_path. The default search path is `~/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles`.
- in the directory where the playbook file is located

You can store the roles in a different location settings `roles_path` in the ansible.cfg

## 12.1 Creating a role (task) <a name="creating_roles"></a>

`/etc/ansible/roles/example/tasks/main.yml`

```yaml
- name: Install service
  # ....
```

## 12.2 Using roles <a name="using_roles"></a>

## 12.2.1 Play level <a name="roles_play_level"></a>

Roles in the session roles run before any other tasks in a play.

```yaml
- hosts: all
  roles:
    - role: '/etc/ansible/roles/example'
```

```yaml
- hosts: all
  roles:
    - my_namespace.my_collection.role1
```

```yaml
- hosts: all
  roles:
    - example
    - common:
      vars:
        dir: '/opt/a'
        app_port: 5000
```

## 12.2.2 Task level <a name="roles_task_level"></a>

**Including roles: dynamic use**

The roles in tasks run in the order they are defined. Variables can be defined in the runtime.

```yaml
- hosts: all
  tasks:
    - name: Include the example role
      include_role:
        name: example
```

**Including roles: static use**

The behavior is the same as using the roles keyword.

```yaml
- hosts: all
  tasks:
    - name: Import the example role
      import_role:
        name: example
```

## 12.2.3 Running specific task files <a name="roles_specific_task"></a>

You can avoid running the default `main.yml` by using `tasks_from`

```yaml
- name: Run specific task from the role
  include_role:
    name: example
    tasks_from: your_tasks_file.yml
```

Another approach is to modify main.yml only to run certain tasks when a condition is met.

```yaml
# In main.yml
- name: Run specific tasks
  include_tasks: your_tasks_file.yml
  when: some_condition
```


# 13 Vault <a name="vault"></a>

[[doc]](https://docs.ansible.com/ansible/latest/vault_guide/index.html)

Ansible Vault is a feature of Ansible that allows you to keep sensitive data such as passwords or keys in encrypted files or variables, it can encrypt any data file used by Ansible. To use Ansible Vault you need one or more passwords to encrypt or decrypt content.

The command to use Ansible Vault is `ansible-vault` and the available arguments are `create,decrypt,edit,view,encrypt,encrypt_string,rekey`.

## 13.1 Vault Password <a name="vault_password"></a>

Running any `ansible-vault` command you will prompted for a password. To enforce password prompt when running a playbook with `ansible-playbook` command, you can add the argument `--ask-vault-pass` to the command line.

The vault password can be stored:
- In a file:
  - Adding the argument `--vault-password-file /path/to/vault_password` to the command line.
  - Setting the file path in the ansible.cfg file updating the entry `vault_password_file=`.
  - Setting the file path in the environment variable `ANSIBLE_VAULT_PASSWORD_FILE`.
- In third-party tools with client scripts.
  - `ansible-playbook --vault-password-file client.py`
  - `ansible-playbook --vault-id dev@client.py`

## 13.2 Variable-level encryption <a name="vault_variable_level"></a>

Variable-level encryption only works with variables and keeps your files still legible. You can mix plaintext and encrypted variables. However, password rotation is not possible to do with the `rekey` command.

### 13.2.1 Encrypting variables <a name="vault_variable_encryption"></a>

**Example**: Encrypting the string '1234' using the variable name 'my_secret'

Command:

```yaml
ansible-vault encrypt_string '1234' --name 'my_secret'
```

Output:

```yaml
my_secret: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          30656465653266303161616131336562316661656331356231633330343131626264643864313335
          3031656537383066363337383964646462383938336630650a626330633762356266313366373464
          62386332653766316462343530323832303432353738313265633766653263633035313034313963
          3138613132386239660a383365393161323363383061353866656639633732326465336261646662
          6239
```

### 13.2.2 Viewing encrypted variables <a name="view_encryt_variable"></a>

You can view the original value using the debug module.

Command:

```yaml
ansible localhost -m ansible.builtin.debug -a var="my_secret" -e "@variables.yml" --ask-vault-pass
```

Output:

```yaml
localhost | SUCCESS => {
    "my_secret": "1234"
}
```

## 13.3 File-level encryption <a name="vault_file_level"></a>

File-level encryption is easy to use, encrypting variables, tasks, or other Ansible content files. It also allows password rotation with `rekey`, but all the file content will be encrypted, you will not 
be able to read the variable name without decrypting it.

### 13.3.1 Encrypting files <a name="vault_encrypting_files"></a>

Variable file content (variables.yml):
```yaml
username: 'leo'
password: '1234'
```

Command:
```yaml
ansible-vault encrypt variables.yml      # Encrypt an existing file
ansible-vault create variables.yml       # Optionaly this command open a text editor creating a new encrypted file
```

Encrypted file content:
```yaml
$ANSIBLE_VAULT;1.1;AES256
61663463663838653230363163373233323739663930396238346462666466663332373334396537
3932623330646639653130316530353937666634643638350a616238653639363334623437353337
33306234353239656264643633613938316537626237653264383161326635623962383630383363
3064383438663031630a663963333937393464326335356666323733366230313531613431336135
61373930343333303665363532656634376339373637626466353436626633343863313566323665
3166353736346239346166346166393530373532616231343530
```

### 13.3.2 Decrypting files <a name="vault_decrypting_files"></a>

```yaml
ansible-vault decrypt variables.yml      # Decrypt the entire file
ansible-vault view variables.yml         # View the content decrypted
ansible-vault edit variables.yml         # Open a editor to edit the 
```

### 13.3.3 Rotating password <a name="vault_rotating_password"></a>

```yaml
ansible-vault rekey variables.yml        # Change the ecryption key
```

## 13.4 Vault ID - Multiple passwords <a name="vault_id"></a>

You can encrypt files and variables with different passwords. For that you can specify an label (Vault ID) for each encrypted content, using the argument `--vault-id`.

In additonal to the label, you must provide a source.

```yaml
--vault-id label@source
```

Kind of sources:

```yaml
--vault-id leo@prompt       # The password will be prompted
--vault-id leo@my_pass      # Password file called 'my_pass'
--vault-id leo@client.py    # Third-party tool script
```

**Examples**

```yaml
ansible-vault encrypt vars.yaml --vault-id leo@prompt                   # Encrypting with label
ansible-playbook hello.yml --vault-id leo@prompt                        # Run playbook asking for the password of the label 'leo'
ansible-playbook hello.yml --vault-id leo@prompt --vault-id dev@prompt  # Asing multiple passwords
```

> **_NOTE 1:_** You can encrypt contents with different passwords for the same label (Vault ID), Anisble does not validate it.

> **_NOTE 2:_** Even if the label is wrong, the decryption will work if the password is right. Ansible will try to decrypt files/variables with any password given, first trying to do it with the password of the matching label to increase the performance.

# 14 Collection <a name="collection"></a>

[doc](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_structure.html)

A collection is a data structure that can contain these directories and files:

```
collection/
    docs/
    galaxy.yml
    meta/
        runtime.yml
    plugins/
        modules/
            module1.py
        inventory/
        .../
    README.md
    roles/
        role1/
        role2/
        .../
    playbooks/
        files/
        vars/
        templates/
        tasks/
    tests/
```

**galaxy.yml**

A collection must have a galaxy.yml file that contains the necessary information to build a collection artifact. [See Collection Galaxy metadata structure](https://docs.ansible.com/ansible/latest/dev_guide/collections_galaxy_meta.html#collections-galaxy-meta) for details.

# 15 Developing Modules <a name="developing_modules"></a>

[[doc]](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html) [[Should you develop a module?]](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules.html#developing-modules)

If you need functionality that is not available in any of the thousands of Ansible modules found in collections, you can easily write your own custom module. Modules can be written in any language, but must of guides use Python. 

This guide helps you to develop Python modules.

Start copying the [Custom Module Template](https://github.com/leofds/notes/tree/master/ansible/custom_module_template.py) file to your workspace `$HOME/library/my_test.py`, modify and extend the code to do what you want.

Ansible won't find this module automatically, for that you have these options:

- Put your module in the default module path `.ansible/plugins/modules/` by creating the directories first: `mkdir -p $HOME/.ansible/plugins/modules/`. The default module path can be changed in the ansible.cfg file.
- Set the environment variable `export ANSIBLE_LIBRARY="$HOME/library"`

Module [Return Values](https://docs.ansible.com/ansible/latest/reference_appendices/common_return_values.html)


## 15.1 Verifying your module locally <a name="verify_your_module"></a>

### 15.1.1 Using Ansible adhoc command <a name="verify_your_module_adhoc_command"></a>

Command

```yaml
ansible localhost -m my_test -a 'name=hello new=true'
```

Output

```shell
localhost | CHANGED => {
    "changed": true,
    "message": "goodbye",
    "original_message": "hello"
}
```

### 15.1.2 Using the module in a Playbook <a name="verify_your_module_in_a_playbook"></a>

Create the playbook file `testmod.yml`

```yaml
- name: test my new module
  hosts: localhost
  gather_facts: false
  tasks:
  - name: run the new module
    my_test:
      name: 'hello'
      new: true
    register: testout
  - name: dump test output
    debug:
      msg: '{{ testout }}'
```

Command

```yaml
ansible-playbook testmod.yml
```

Output

```shell
PLAY [test my new module] **********************************************************************************************************************************

TASK [run the new module] **********************************************************************************************************************************
changed: [localhost]

TASK [dump test output] ************************************************************************************************************************************
ok: [localhost] => {
    "msg": {
        "changed": true,
        "failed": false,
        "message": "goodbye",
        "original_message": "hello"
    }
}

PLAY RECAP *************************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### 15.1.3 Using Python <a name="verify_your_module_using_python"></a>

Create a JSON file `/tmp/args.json`.

```json
{
    "ANSIBLE_MODULE_ARGS": {
        "name": "hello",
        "new": true
    }
}
```

Command

```shell
source $HOME/.local/share/pipx/venvs/ansible/bin/activate  # Only if ansible installed with pipx

python3 library/my_test.py /tmp/args.json
```

Output

```json
{
  "changed": true,
  "original_message": "hello",
  "message": "goodbye",
  "invocation": {
    "module_args": {
      "name": "hello",
      "new": true
    }
  }
}
```
# 16 Developing Collections <a name="developing_collections"></a>

[[doc]](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections_creating.html) [[Creating a new collection]](https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_in_groups.html#developing-modules-in-groups)

Collections are a distribution format for Ansible content. You can package and distribute playbooks, roles, modules, and plugins using collections.

To create a collection:

1. Create a new collection with a template running the command: 

```shell
ansible-galaxy collection init my_namespace.my_collection
```
2. Add modules and other content to the collection, and edit the file `galaxy.yml`.

3. Install or build collection:

```shell
# Install collection
ansible-galaxy collection install path/to/my_namespace/my_collection

# Build collection
ansible-galaxy collection build path/to/my_namespace/my_collection
ansible-galaxy collection install path/to/my_namespace-my_collection-1.0.0.tar.gz  # Install builded collection
```

4. (Optional) Publish the collection artifact to Galaxy:

```shell
ansible-galaxy collection publish path/to/my_namespace-my_collection-1.0.0.tar.gz --api-key=SECRET
```

You can change the default collections path in the ansible.cfg file by changin the property `collections_path=`.

# 17 Developing Plugins <a name="developing_plugins"></a>

Developing Ansible plugins allows you to extend Ansible's functionality beyond what's available by default.

Ansible supports several plugin types:

- Action Plugin
- Cache Plugin
- Callback Plugin
- Connection Plugin
- Filter Plugin
- Inventory Plugin
- Lookup Plugin
- Test Plugin
- Vars Plugin

Check the page [Developing Plugins](https://github.com/leofds/ansible-essentials/blob/master/developing_plugins.md)

## 18 Env Vars <a name="env_vars"></a>

```shell
ANSIBLE_DEBUG=1    
```

## 19 Troubleshooting <a name="troubleshooting"></a>

```shell
ansible-config dump | grep CALLBACK      # Check if the callback plugin was loaded
```
