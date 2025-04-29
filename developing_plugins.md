# Developing Plugins

[[doc]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html)
[[Working with plugins]](https://docs.ansible.com/ansible/latest/plugins/plugins.html)

**Ansible Display Class**

This class controls how messages are printed to the screen:

```python
display = Display()
display.display("This is a normal message")
display.error("This is an error message!")
display.warning("This is a warning!")
display.debug("This is a debug message only show with -vvv")
display.vvv("This will only show with -vvv")
display.banner("=== PLAYBOOK FINISHED ===")
```

## 1 Action Plugin <a name="action_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/action.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#action-plugins)

## 2 Cache Plugin <a name="cache_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/cache.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#cache-plugins)

## 3 Callback Plugin <a name="callback_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/callback.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#callback-plugins)
[[examples]](https://github.com/ansible/ansible/tree/devel/lib/ansible/plugins/callback)

A callback plugin is a type of plugin that allows you to customize how Ansible handles and displays output during playbook execution. Callback plugins can be used to log results, send notifications, format output differently, or integrate Ansible with external tools.

Create the callback plugin file and enable it the `ansible.cfg`:

1. Set the directory path in the field: `callback_plugins=/path/to/callback_directory/`
2. Enable the callback file in the field: `callbacks_enabled=my_callback`
3. chmod 644 `/path/to/callback_directory/my_callback.py`

`CALLBACK_TYPE`

The CALLBACK_TYPE in an Ansible callback plugin defines how the plugin interacts with Ansible. It categorizes the plugin's purpose and determines when Ansible will invoke it.

- `stdout` Affects how playbook output is displayed.
- `notification` Sends results to an external system.
- `aggregate` Gathers and processes data from multiple tasks before outputting results.
- `action` Runs additional actions based on task execution (less common).

**Example 1** type stdout

You can only have one stdout callback at a time. Change it in the `ansible.cfg` field `stdout_callback=my_callback`.

```python
from ansible.plugins.callback import CallbackBase

class CallbackModule(CallbackBase):
    CALLBACK_VERSION = 2.0
    CALLBACK_TYPE = 'stdout'
    CALLBACK_NAME = 'my_callback'

    def v2_runner_on_ok(self, result):
        print(f"TASK SUCCESS: {result._task.get_name()} -> {result._result}")

    def v2_runner_on_failed(self, result, ignore_errors=False):
        print(f"TASK FAILED: {result._task.get_name()} -> {result._result}")

    def v2_runner_on_skipped(self, result):
        print(f"TASK SKIPPED: {result._task.get_name()} -> {result._result}")

    def v2_runner_on_unreachable(self, result):
        print(f"TASK UNREACHABLE: {result._task.get_name()} -> {result._result}")
```

**Example 2** type notification

Aborting the playbook execution.

```python
from ansible.plugins.callback import CallbackBase
from ansible.utils.display import Display

class CallbackModule(CallbackBase):
    CALLBACK_VERSION = 2.0
    CALLBACK_TYPE = 'notification'
    CALLBACK_NAME = 'my_callback'

    def __init__(self):
        super(CallbackModule, self).__init__()
        self._display = Display()

    def v2_playbook_on_play_start(self, play):
        self._display.error(f"Execution blocked")
        raise SystemExit("Playbook execution aborted")
```

## 4 Connection Plugin <a name="connection_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/connection.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#connection-plugins)

## 5 Filter Plugin <a name="filter_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/filter.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#filter-plugins)

**Filter sample**

Create the filter plugin file:

1. In the collection `path/to/my_namespace/my_collection/plugins/filter/string.py`.
2. Or in another directory by changing the `ansible.cfg` field `filter_plugins=/path/to/filter_directory/`.

```python
class FilterModule(object):
    def filters(self):
        return {
            'to_upper': self.to_upper,
        }

    def to_upper(self, value):
        if isinstance(value, str):
            return value.upper()
        else:
            raise ValueError("The value must have a string")
```

**Using the filter**

```python
- hosts: localhost
  tasks:
    - name: Test the custom filter
      ansible.builtin.debug:
        msg: "{{ 'hello world' | to_upper }}"
        # msg: "{{ 'hello world' | my_namespace.my_collection.to_upper }}"
```

## 6 Inventory Plugin <a name="inventory_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/inventory.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#inventory-plugins)

## 7 Lookup Plugin <a name="lookup_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/lookup.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#lookup-plugins)

## 8 Test Plugin <a name="test_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/test.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#test-plugins)

## 9 Vars Plugin <a name="vars_plugin"></a>

[[doc]](https://docs.ansible.com/ansible/latest/plugins/vars.html)
[[dev]](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html#vars-plugins)
