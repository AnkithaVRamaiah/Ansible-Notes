# Variables

Using **variables** in Ansible playbooks makes them dynamic and reusable by allowing you to pass values and parameters that can be adjusted without modifying the core playbook. Here are some key aspects of variables in Ansible playbooks:

### 1. **Defining Variables**
You can define variables in several places:
- **In the playbook:** Directly within the playbook file.
- **In separate variable files:** Using a `vars_files` section or by including a `vars` directory.
- **From the command line:** Using `-e` (extra vars) option.
- **In inventory files:** Group and host-specific variables.
- **In the `defaults` directory:** For role-specific defaults.

#### Example:
```yaml
---
- name: Install and configure Nginx
  hosts: webservers
  vars:
    nginx_port: 80
    nginx_user: "www-data"
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```

### 2. **Using Variables**
You can use the variables by referencing them with `{{ variable_name }}`.

#### Example:
```yaml
- name: Ensure Nginx is running
  service:
    name: nginx
    state: started
    enabled: yes
```

Here, `nginx` is the service name, and the state is controlled using a variable.

### 3. **Defining Variables in Inventory**
You can define variables in your inventory files, either for groups of hosts or for individual hosts.

#### Example:
```ini
[webservers]
web1 ansible_host=192.168.1.10 nginx_port=8080
web2 ansible_host=192.168.1.11 nginx_port=8080

[dbservers]
db1 ansible_host=192.168.1.20 db_user=root
```

### 4. **Using External Variable Files**
You can include external files for variables.

#### Example:
```yaml
vars_files:
  - vars/common.yml
  - vars/webservers.yml
```

In this case, `common.yml` and `webservers.yml` contain the variables that can be reused across different playbooks.

### 5. **Overriding Variables**
Variables can be overridden in various ways:
- Command line `-e` can override any variable.
- Host and group variables in the inventory file can override playbook variables.

### 6. **Looping with Variables**
Variables are particularly useful in loops to handle dynamic lists.

#### Example:
```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - curl
    - git
```

### 7. **Jinja2 Filters**
You can manipulate variables using Jinja2 filters to format or modify values.

#### Example:
```yaml
- name: Display the current date
  debug:
    msg: "Today's date is {{ ansible_date_time.iso8601 | regex_replace('T.*', '') }}"
```

This example formats the `ansible_date_time` variable to show only the date part.

### Conclusion
Using variables allows you to make your Ansible playbooks dynamic, flexible, and reusable. By setting variables for environment-specific configurations, you can adapt your playbooks for different setups without needing to rewrite them.

# Gathering Facts

In Ansible, you can gather facts about managed hosts using the **`setup`** module, which collects various system information (facts) from the target machines. These facts can then be used in your playbooks for conditional tasks, logging, or other configurations.

### 1. **Gathering Facts**

By default, Ansible collects facts when you run a playbook, but you can manually gather them by using the `setup` module.

```yaml
- name: Gather facts about managed hosts
  hosts: all
  tasks:
    - name: Collect system facts
      setup:
```

### 2. **Filtering Specific Facts**

You can filter the facts to gather only specific information using the `gather_subset` option. This can optimize performance when you don't need all the details.

```yaml
- name: Gather only specific facts
  hosts: all
  tasks:
    - name: Collect network facts
      setup:
        gather_subset:
          - network
```

### 3. **Using Gathered Facts in Playbooks**

Once the facts are collected, you can use them in your playbook tasks. Here's an example of using gathered facts:

```yaml
- name: Gather facts and use them in tasks
  hosts: all
  tasks:
    - name: Collect system facts
      setup:
    
    - name: Display the OS family
      debug:
        msg: "The OS family is {{ ansible_facts['os_family'] }}"
    
    - name: Install a package based on OS
      package:
        name: "{{ 'httpd' if ansible_facts['os_family'] == 'RedHat' else 'apache2' }}"
        state: present
```

### 4. **Common Facts Collected**

Here are some of the common facts gathered by the `setup` module:

- `ansible_facts['hostname']` - The hostname of the managed host.
- `ansible_facts['os_family']` - The operating system family (e.g., RedHat, Debian).
- `ansible_facts['distribution']` - The name of the OS (e.g., CentOS, Ubuntu).
- `ansible_facts['architecture']` - The system architecture (e.g., x86_64).
- `ansible_facts['memory']` - The memory information (e.g., total memory, free memory).
- `ansible_facts['cpu']` - The CPU details (e.g., number of processors, model).
- `ansible_facts['ip_interfaces']` - Network interfaces and their IP addresses.

### 5. **Using Facts for Conditional Execution**

You can use the gathered facts to run tasks conditionally. For example, using `ansible_facts['distribution']` to execute specific tasks on different Linux distributions:

```yaml
- name: Conditional tasks based on distribution
  hosts: all
  tasks:
    - name: Install package on RedHat-based systems
      package:
        name: httpd
        state: present
      when: ansible_facts['distribution'] == 'CentOS'
    
    - name: Install package on Debian-based systems
      package:
        name: apache2
        state: present
      when: ansible_facts['distribution'] == 'Ubuntu'
```

### 6. **Disabling Fact Gathering**

If you don’t need the facts for a specific playbook, you can disable fact gathering to speed up the playbook execution:

```yaml
- name: Disable fact gathering
  hosts: all
  gather_facts: no
  tasks:
    - name: Run without facts
      debug:
        msg: "This playbook doesn't gather facts."
```

By gathering and using facts, you can make your playbooks more dynamic and flexible, allowing them to adapt based on the environment they are executed in.