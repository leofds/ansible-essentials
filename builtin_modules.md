# Ansible.Builtin modules

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)

Back to [Ansible](https://github.com/leofds/notes/tree/master/ansible/ansible.md)

> **_NOTE:_** Prefer to use a Fully Qualified Collection Name (FQCN) like `ansible.builtin.debug` instead of `debug`, for linking with module documentation and to avoid conflicts with the other collections that may have the same module name.

# Summary

1. [Debug](#debug)
2. [Shell](#shell)
3. [Stat](#stat)
4. [Copy](#copy)
5. [File](#file)
6. [Fetch](#fetch)
7. [Find](#find)
8. [URI](#uri)
9. [Add Host](#add_host)
10. [Pause](#pause)
11. [Fail](#fail)
12. [Set Fact](#set_fact)
13. [Assert](#assert)
14. [Wait for connection](#wait_for_conn)
15. [Wait_for](#wait_for)
16. [Get_URL](#get_url)
17. [Replace](#replace)
18. [Reboot](#Reboot)
19. [Import tasks](#import_tasks)

# 1. Debug <a name="debug"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/debug_module.html)

This module prints variables or expressions.

```yaml
- name: Print the content of the variable username
  ansible.builtin.debug:
    msg: User {{ username }}
```

```yaml
- name: Print return information from the previous task
  ansible.builtin.debug:
    var: result
    verbosity: 2
```

# 2. Shell <a name="shell"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html)

This module runs the command through a shell (/bin/sh) on the remote node.

```yaml
- name: Get uptime information
  ansible.builtin.shell: /usr/bin/uptime
  register: result
```

# 3. Stat <a name="stat"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/stat_module.html)

Retrieves facts for a file similar to the Linux/Unix ‘stat’ command.

```yaml
- name: Get stats of a file
  ansible.builtin.stat:
    path: /etc/foo.conf
  register: st

- name: Debug
  ansible.builtin.debug:
    var: st.stat
```

<details>

<summary>Debug output</summary>

```json
ok: [localhost] => {
    "st.stat": {
        "atime": 1721699571.7296615,
        "attr_flags": "",
        "attributes": [],
        "block_size": 4096,
        "blocks": 0,
        "charset": "us-ascii",
        "checksum": "f572d396fae9206628714fb2ce00f72e94f2258f",
        "ctime": 1721699571.7296615,
        "dev": 2,
        "device_type": 0,
        "executable": false,
        "exists": true,
        "gid": 1000,
        "gr_name": "leo",
        "inode": 10133099162624209,
        "isblk": false,
        "ischr": false,
        "isdir": false,
        "isfifo": false,
        "isgid": false,
        "islnk": false,
        "isreg": true,
        "issock": false,
        "isuid": false,
        "mimetype": "text/plain",
        "mode": "0644",
        "mtime": 1721699571.7296615,
        "nlink": 1,
        "path": "/home/leo/foo.conf",
        "pw_name": "leo",
        "readable": true,
        "rgrp": true,
        "roth": true,
        "rusr": true,
        "size": 6,
        "uid": 1000,
        "version": null,
        "wgrp": false,
        "woth": false,
        "writeable": true,
        "wusr": true,
        "xgrp": false,
        "xoth": false,
        "xusr": false
    }
}
```

</details>

</br>

Checking if the file exists

```yaml
- name: Check if the file not exists
  ansible.builtin.debug:
    msg: 'Not exist'
  when: not st.stat.exists
```

# 4. Copy <a name="copy"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html)

This module copies a file or a directory structure from the local or remote machine to a location on the remote machine.

```yaml
- name: Copy file from the controller to remote with owner and permissions
  ansible.builtin.copy:
    src: /srv/myfiles/foo.conf
    dest: /etc/foo.conf
    owner: foo
    group: foo
    mode: '0644'

- name: Copy file on the remote
  ansible.builtin.copy:
    src: /tmp/foo.conf
    dest: /etc/foo.conf
    owner: foo
    group: foo
    mode: '0644'
    remote_src: yes

- name: Create file with content
  ansible.builtin.copy:
    dest: /path/to/foo.txt
    content: |
      This is the file content.
      It can include multiple lines.
```

# 5. File <a name="file"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html)

Set attributes of files, directories, or symlinks and their targets. Alternatively, remove files, symlinks or directories.

```yaml
- name: Create a directory if it does not exist
  ansible.builtin.file:
    path: /etc/some_directory
    owner: root
    group: root
    mode: '0755'
    state: directory

- name: Create a symbolic link
  ansible.builtin.file:
    src: /file/to/link/to
    dest: /path/to/symlink
    owner: foo
    group: foo
    state: link

- name: Change file ownership, group and permissions
  ansible.builtin.file:
    path: /etc/foo.conf
    owner: foo
    group: foo
    mode: '0644'

- name: Recursively change ownership of a directory
  ansible.builtin.file:
    path: /etc/foo
    state: directory
    recurse: yes
    owner: foo
    group: foo

- name: Remove file (delete file)
  ansible.builtin.file:
    path: /etc/foo.txt
    state: absent

- name: Recursively remove directory
  ansible.builtin.file:
    path: /etc/foo
    state: absent
```
# 6. Fetch <a name="fetch"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/fetch_module.html)

This module works like `ansible.builtin.copy`, but in reverse. It is used for fetching files from remote machines and storing them locally.

```yaml
- name: Specifying a destination path
  ansible.builtin.fetch:
    src: /tmp/uniquefile
    dest: /tmp/special/
    flat: yes
```
# 7. Find <a name="find"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/find_module.html)

Return a list of files based on specific criteria.

```shell
- name: Find /var/log all directories
  ansible.builtin.find:
    paths: /var/log
    recurse: no
    file_type: directory
```

# 8. URI <a name="uri"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html)

Interacts with HTTP and HTTPS web services and supports Digest, Basic and WSSE HTTP authentication mechanisms

```yaml
- name: Retry combined with a loop
  uri:
    url: "https://{{ item }}.ansible.com"
    method: GET
  register: uri_output
  with_items:
  - "galaxy"
  - "docs"
  - "forum"
  - "www"
  retries: 2
  delay: 1
  until: "uri_output.status == 200"

- name: Show the output
  debug:
    msg: "{{ uri_output }}"
```

# 9. Add Host <a name="add_host"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/add_host_module.html)

Create new hosts and groups in inventory for use in later plays of the same playbook.

```yaml
- name: Set remote host dynamically
  ansible.builtin.add_host:
    name: remote_host
    ansible_host: "{{ remote_ip }}"
    ansible_ssh_user: "{{ remote_user }}"
    ansible_ssh_pass: "{{ remote_password }}"
    ansible_ssh_common_args: "-o UserKnownHostsFile=/dev/null -o PreferredAuthentications=password -o PubkeyAuthentication=no"
    # ansible_ssh_extra_args: "-o UserKnownHostsFile=/dev/null -o PreferredAuthentications=password -o PubkeyAuthentication=no"

- name: Ping Remote host
  ansible.builtin.ping:
  delegate_to: remote_host
```

# 10. Pause <a name="pause"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/pause_module.html)

```yaml
- name: Pause to get some sensitive input
  ansible.builtin.pause:
    prompt: "Enter a secret"
    echo: no
```

# 11. Fail <a name="fail"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/fail_module.html)

```yaml
- name: Example using fail and when together
  ansible.builtin.fail:
    msg: The system may not be provisioned according to the CMDB status.
  when: cmdb_status != "to-be-staged"
```

# 12. Set Fact <a name="set_fact"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/set_fact_module.html)

This action allows setting variables associated to the current host. These variables will be available to subsequent plays during an ansible-playbook that have the same host.

```yaml
- name: Update a variable
  ansible.builtin.set_fact:
    my_variable: 'hello'
```

# 13. Assert <a name="assert"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/assert_module.html)

```shell
- name: A single condition can be supplied as string instead of list
  ansible.builtin.assert:
    that: "ansible_os_family != 'RedHat'"
```

# 14. Wait for connection <a name="wait_for_conn"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/wait_for_connection_module.html)

- Waits for a total of `timeout` seconds.
- Retries the transport connection after a `timeout` of `connect_timeout`.
- Tests the transport connection every `sleep seconds`.

```yaml
- name: Wait 300 seconds, but only start checking after 60 seconds
  ansible.builtin.wait_for_connection:
    delay: 60
    timeout: 300
```
# 15. Wait for <a name="wait_for"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/wait_for_module.html)

```yaml
- name: Sleep for 300 seconds and continue with play
  ansible.builtin.wait_for:
    timeout: 300
  delegate_to: localhost

- name: Wait for port 8000 to become open on the host, don't start checking for 10 seconds
  ansible.builtin.wait_for:
    port: 8000
    delay: 10
```

# 16. Get URL <a name="get_url"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html)

Downloads files from HTTP, HTTPS, or FTP to the remote server. The remote server must have direct access to the remote resource.

```yaml
- name: Download file
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    mode: '0440'
```

# 17. Replace <a name="replace"></a>

[[doc]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/replace_module.html)

This module will replace all instances of a pattern within a file.

```yaml
- name: Replace
  ansible.builtin.replace:
    path: /etc/hosts
    regexp: '^value=""'
    replace: 'value="dummy"'
```

# 18. Reboot <a name="reboot"></a>

[[reboot_module]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/reboot_module.html)

Reboot a machine, wait for it to go down, come back up, and respond to commands..

```yaml
- name: Reboot the machine
  ansible.builtin.reboot:
    reboot_timeout: 3600      # Maximum seconds to wait for machine to reboot and respond to a test command.
```

# 19. Import tasks <a name="import_tasks"></a>

[[ansible.builtin.import_tasks module]](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_tasks_module.html)

Imports a list of tasks to be added to the current playbook for subsequent execution.

```yaml
- hosts: all
  tasks:
    - name: Include task list in play
      ansible.builtin.import_tasks:
        file: stuff.yaml

    - name: Apply conditional to all imported tasks
      ansible.builtin.import_tasks: stuff.yaml
      when: hostvar is defined
```
