### **Ansible Roadmap**
#### **1. Introduction to Ansible**
- Understand what Ansible is and its key features.
- Learn about the **agentless architecture** and how it differs from other configuration management tools.
- Install Ansible on your local machine (Linux or macOS preferred).

#### **2. Ansible Basics**
- **Ansible Inventory**: Learn how to define hosts and groups of hosts.
- **Ad-hoc Commands**: Execute simple tasks using Ansible ad-hoc commands.
- **Modules**: Explore commonly used Ansible modules (e.g., `ping`, `command`, `copy`, `service`).

#### **3. Writing Playbooks**
- Understand what **playbooks** are and how they structure tasks.
- Learn about **YAML** syntax, as Ansible playbooks are written in YAML.
- Create simple playbooks to automate basic tasks (e.g., installing packages, starting services).

#### **4. Variables and Facts**
- Use **variables** to make playbooks more dynamic and reusable.
- Gather **facts** about managed hosts and use them in playbooks.

#### **5. Handlers and Conditions**
- Learn how to use **handlers** to perform actions when tasks change the state.
- Implement **conditional statements** to run tasks based on certain conditions.

#### **6. Loops and Templates**
- Use **loops** to repeat tasks with different inputs.
- Create **Jinja2 templates** for generating configuration files dynamically.

#### **7. Roles**
- Understand the concept of **roles** for organizing playbooks and tasks.
- Create roles to structure and reuse your Ansible code.

#### **8. Ansible Galaxy**
- Explore **Ansible Galaxy** for downloading pre-built roles.
- Learn how to upload and manage your own roles on Galaxy.

#### **9. Ansible Vault**
- Use **Ansible Vault** to securely manage sensitive data (e.g., passwords, API keys).
- Encrypt and decrypt data within your playbooks.

#### **10. Advanced Playbook Features**
- Implement **tags** to control which tasks run.
- Use **block**, **rescue**, and **always** for better error handling.
- Optimize playbooks with **delegate_to**, **run_once**, and **local_action**.

#### **11. Ansible Tower/AWX**
- Learn about **Ansible Tower** or **AWX** for managing Ansible playbooks with a web interface.
- Understand the features like **RBAC (Role-Based Access Control)**, **job templates**, and **workflow automation**.

#### **12. Ansible for DevOps**
- Integrate Ansible with **CI/CD pipelines**.
- Automate **infrastructure provisioning** (e.g., AWS, Azure).
- Use Ansible for **container orchestration** and **Kubernetes** management.

#### **13. Best Practices**
- Follow **best practices** for writing maintainable and scalable Ansible playbooks.
- Learn about **idempotency**, which ensures tasks don't make unnecessary changes.
- Use **Ansible Lint** to check for syntax and best practice errors in playbooks.

#### **14. Real-World Projects**
- Implement real-world automation scenarios like setting up web servers, configuring databases, or deploying applications.
- Participate in open-source projects or contribute to Ansible Galaxy.

#### **15. Certification (Optional)**
- Consider taking the **Red Hat Certified Specialist in Ansible Automation** exam for formal recognition of your skills.

---

# conditions

To implement **conditional statements** in your shell script, you can use basic constructs like `if`, `else`, `elif`, and `test` commands. Here's an example of how you can use conditional statements to run tasks based on certain conditions.

### Example: Shell Script with Conditional Statements

```bash
#!/bin/bash
# Author: Ankita V
# Date: 2025-01-21
# Version: 1.0
# Purpose: A script to check the system's disk usage and take action based on conditions

# Get the disk usage percentage of the root filesystem
disk_usage=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

# Check if disk usage is above 90%
if [ $disk_usage -gt 90 ]; then
    echo "Warning: Disk usage is above 90%! Running cleanup task..."
    # Add your cleanup task here, for example:
    # sudo rm -rf /path/to/temp/files
elif [ $disk_usage -gt 75 ]; then
    echo "Disk usage is between 75% and 90%. Please monitor it."
else
    echo "Disk usage is normal."
fi

# Check if a certain process is running
process_name="apache2"
if pgrep $process_name > /dev/null; then
    echo "$process_name is running."
else
    echo "$process_name is not running. Starting the process..."
    # Start the service if not running
    sudo systemctl start $process_name
fi
```

### Explanation:

1. **Disk Usage Check:**
   - The script checks the disk usage of the root filesystem (`/`) using the `df` command.
   - It then uses conditional statements to determine if the disk usage is above 90% (warning), between 75% and 90% (monitor), or normal.
   - Based on the result, it runs different tasks, such as a cleanup task when disk usage is over 90%.

2. **Process Check:**
   - The script checks if a specific process (e.g., `apache2`) is running using the `pgrep` command.
   - If the process is not running, it starts the service using `systemctl`.

### How Conditional Statements Work:
- `if [ condition ]; then` – checks if the condition is true and runs the block of code.
- `elif [ condition ]; then` – checks another condition if the first one is false.
- `else` – runs the block of code when no other conditions are true.
- `pgrep` – checks if a process is running.
- `-gt` – checks if a value is greater than another value.

This structure helps automate tasks based on system conditions.