---
marp: true
title: Automate Checkmk with Ansible
theme: rk-it
size: 16:9
paginate: true
footer: Checkmk Conference #12, 18.06.2026
---

# Automate Checkmk with Ansible

---

<!-- _class: right-bg-author -->
# 👋 About me

- René Koch
- Self-employed consultant for:
  - Red Hat Ansible (Automation Platform)
  - Red Hat Enterprise Linux
  - Red Hat Satellite
  - Red Hat Identity Management (IPA)
- Experienced monitoring user (Nagios, Icinga,
  Checkmk)

---

<!-- _class: right-bg-author -->
# 👋 About me

- René Koch
  - rkoch@rk-it.at
  - +43 660 / 464 0 464
  - https://www.linkedin.com/in/rk-it-at
  - https://github.com/rk-it-at
  - https://github.com/scrat14

---

# 👥 Please introduce yourself

---

# 🕘 Timetable

- 09:00 – 10:30: Workshop
- 10:30 – 10:45: Break
- 10:45 – 12:30: Workshop
- 12:30 – 13:45: Lunch break
- 13:45 – 15:30: Workshop
- 15:30 – 15:45: Break
- 15:45 – 17:15: Workshop

---

# 🧭 Table of contents

- 🧪 Prepare LAB environment
- 🖥️ Installation of Checkmk server
- 💻 Installation of Checkmk agents
- 🔧 Checkmk server configuration
- 🔎 Fetch information with lookup plugins
- 📚 Using Checkmk as Ansible inventory
- 🚀 Demo: Self-healing with Checkmk, Ansible Automation Platform and rulebooks

---

# 🧪 Preparing the LAB environment

---

# 🧪 Preparing the LAB environment

- 🛠️ Create LAB environment
- 🧩 Ansible control node =
      Checkmk server =
      Checkmk agent
- 🔌 Use local connection
- 🔐 Configure sudo for privileged access

---

# 🧪 Preparing the LAB environment

- 🔗 Slides and code are available on GitHub:
  https://github.com/rk-it-at/checkmk_conference_2026
- ➕ Optional: install and clone the repository:
```bash
git clone https://github.com/rk-it-at/checkmk_conference_2026.git
```

---

# 🧪 Preparing the LAB environment

- 🖥️ Available VMs (if you do not have your own)
- 🔐 Passwords: <TODO>

---

<!-- _class: code-small -->
# 🧪 Preparing the LAB environment

| Hostname | IP Address | Username | Participant |
| --- | --- | --- | --- |
| ansible-ws-1 |  | ansible |  |
| ansible-ws-2 |  | ansible |  |
| ansible-ws-3 |  | ansible |  |
| ansible-ws-4 |  | ansible |  |
| ansible-ws-5 |  | ansible |  |
| ansible-ws-6 |  | ansible |  |
| ansible-ws-7 |  | ansible |  |

---

<!-- _class: code-small -->
# 🧪 Preparing the LAB environment

| Hostname | IP Address | Username | Participant |
| --- | --- | --- | --- |
| ansible-ws-8 |  | ansible |  |
| ansible-ws-9 |  | ansible |  |
| ansible-ws-10 |  | ansible |  |
| ansible-ws-11 |  | ansible |  |
| ansible-ws-12 |  | ansible |  |
| ansible-ws-13 |  | ansible |  |
| ansible-ws-14 |  | ansible |  |
| ansible-ws-15 |  | ansible |  |

---

# 🧪 Preparing the LAB environment

- 🔐 Configure sudo for privileged access
```bash
$ su -
# <EDITOR> /etc/sudoers.d/ansible
```
```
<USERNAME> ALL=(ALL) NOPASSWD: ALL
```

**NOTE**
> Use the root user to run this command!
> Replace <EDITOR> with vi, nano or your favorite editor
> Replace <USERNAME> with your system user

---

# 🧪 LAB 1: Configure sudo

---

# 🧪 LAB 1: Configure sudo

- 🔐 Configure sudo for privileged access
```bash
$ su -
# <EDITOR> /etc/sudoers.d/ansible
```
```
<USERNAME> ALL=(ALL) NOPASSWD: ALL
```

**NOTE**
> Use the root user to run this command!
> Replace <EDITOR> with vi, nano or your favorite editor
> Replace <USERNAME> with your system user

---

# ⚙️ Install Ansible

---

# ⚙️ Install Ansible

- 🐧 Most Linux distributions ship two versions:
  - **ansible-core**: Ansible binary + minimal set of collections
  - **ansible**: Ansible and selected collections

---

# ⚙️ Install Ansible - RHEL

- ⚙️ Install Ansible on RHEL 10

```bash
subscription-manager repos --enable codeready-builder-for-rhel-10-$(arch)-rpms
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-10.noarch.rpm
dnf install ansible
```

---

# ⚙️ Install Ansible – RHEL derivatives

- ⚙️ Install Ansible on AlmaLinux or Rocky Linux 10

```bash
dnf config-manager --set-enabled crb
dnf install epel-release
dnf install ansible
```

---

# ⚙️ Install Ansible – openSUSE

- ⚙️ Install Ansible on openSUSE

```bash
zypper install ansible
```

---

# ⚙️ Install Ansible – Ubuntu

- ⚙️ Install Ansible on Ubuntu 24.04+

```bash
apt update
apt install ansible
```

---

# ⚙️ Install Ansible – pip

- ⚙️ Install Ansible with pip

```bash
pip install ansible
```

---

# 🧪 LAB 2: Install Ansible

---

# 🧪 LAB 2: Install Ansible

- ✅ Ensure Ansible is installed

```bash
ansible --version
```
```
ansible [core 2.16.18]
```

**NOTE**
> Depending on your operating system the Ansible version can be different

---

# 📋 Prepare Ansible inventory

---

# 📋 Prepare Ansible inventory

- 🛠️ Create folder for your Ansible code
```bash
mkdir playbooks
cd playbooks
<EDITOR> hosts
cmk-server ansible_connection=local
cmk-agent ansible_connection=local
```

**NOTE**
> Replace <EDITOR> with vi, nano or your favorite editor

---

<!-- _class: code-small -->
# 🔌 Test connection to target server

- ✅ Make sure your target server is reachable
```yaml
ansible all -i hosts -m ping
cmk-server | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
    }
cmk-agent | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
    }
```

> Ansible does not send an ICMP ping to test the connection, it tries to log into the target machine.

---

# 🧪 LAB 3: Add target to Ansible inventory

---

<!-- _class: code-small -->
# 🧪 LAB 3: Add target to Ansible inventory

- ✅ Make sure your target server is reachable
```yaml
$ ansible all -i hosts -m ping
cmk-server | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
    }
cmk-agent | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
    }
```

---

# 📦 Install Checkmk Collection

---

# 📦 Install Checkmk Collection

- 📦 Simplified and consistent content delivery
- 📦 The collection contains:
  - Modules
  - Playbooks
  - Roles
  - Plugins
  - Docs
  - Tests

---

# 📦 Install Checkmk Collection

- 🧑‍💻 Development:
  https://github.com/Checkmk/ansible-collection-checkmk.general
- 🌌 Hosted at Galaxy:
  https://galaxy.ansible.com/ui/repo/published/checkmk/general
- ⚙️ Install the collection:
```bash
ansible-galaxy collection install checkmk.general
```

---

# 🖥️ Installation of Checkmk server

---

# 🖥️ Installation of Checkmk server

- 📦 Can be installed with the provided server role
- 💡 Strongly recommend writing your own role to fit your Ansible environment and coding guidelines!
- 📚 Role documentation:
  https://galaxy.ansible.com/ui/repo/published/checkmk/general/content/role/server/


---

# 🖥️ Installation of Checkmk server

- 🛠️ Create playbook
```bash
<EDITOR> cmk_server.yml
```

---

# 🖥️ Installation of Checkmk server

```yaml
---

- name: Install Checkmk server
  hosts: cmk-server
  connection: local
  become: true

  roles:
    - checkmk.general.server
```

---

# 🖥️ Installation of Checkmk server

- 🛠️ Create required variables

```bash
mkdir host_vars
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 🖥️ Installation of Checkmk server

```yaml
---
checkmk_server_version: "2.5.0"
checkmk_server_edition: "community"
checkmk_admin_pw: "AnsibleW0rkshop2026!"
checkmk_server_sites:
  - name: "master"
    version: "{{ checkmk_server_version }}"
    update_conflict_resolution: "abort"
    state: started
    admin_pw: "{{ checkmk_admin_pw }}"
```

---

# 🧪 LAB 4: Install Checkmk server

---

# 🧪 LAB 4: Install Checkmk server

- ✅ Ensure Checkmk server is installed
```bash
ansible-playbook -i hosts cmk_server.yml

[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

TASK [checkmk.general.server : Flush Handlers.] **********************************************

...

PLAY RECAP ***********************************************************************************
cmk-server : ok=20 changed=8 unreachable=0 failed=0 skipped=21 rescued=0 ignored=0
```

---

# 🖥️ Installation of Checkmk server

- ✅ Ensure HTTPS is configured
```bash
<EDITOR> cmk_server.yml
```

---

<!-- _class: code-small -->
# 🖥️ Installation of Checkmk server

```yaml
...
  tasks:
    - name: Install mod_ssl for https access
      ansible.builtin.dnf:
        name: mod_ssl
        state: installed
      notify: Restart httpd

  handlers:
    - name: Restart httpd
      ansible.builtin.service:
        name: httpd
        state: restarted
```

---

# 🧪 LAB 5: Secure Checkmk server

---

# 🧪 LAB 5: Secure Checkmk server

- ✅ Ensure Checkmk server is listening to HTTPS

```bash
ansible-playbook -i hosts cmk_server.yml

PLAY RECAP ***********************************************************************************
cmk-server : ok=17 changed=3 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

- 🌐 Access your server: https://<ip_address>/master
- 👤 Username: cmkadmin
- 🔐 Password: AnsibleW0rkshop2026!

---

<!-- _class: footnote-only -->
<!-- _backgroundImage: "url('assets/odp/100000010000078000000500138259B5.png')" -->
<!-- _backgroundSize: auto calc(100% - 3.5cm) -->
<!-- _backgroundPosition: center -->

<div class="corner-logos"></div>

---

# 💻 Installation of Checkmk agent

---

# 💻 Installation of Checkmk agent

- 📦 Can be installed with the provided agent role
- 💡 Strongly recommend writing your own role to fit your Ansible environment and coding guidelines!
- 📚 Role documentation:
  https://galaxy.ansible.com/ui/repo/published/checkmk/general/content/role/agent/

---

# 💻 Installation of Checkmk agent

- 🛠️ Create playbook
```bash
<EDITOR> cmk_agent.yml
```

---

<!-- _class: code-small -->
# 💻 Installation of Checkmk agent

```yaml
---

- name: Install Checkmk agent
  hosts: cmk-agent
  connection: local
  become: true

  pre_tasks:
    - name: Install required Python packages
      ansible.builtin.dnf:
        name: python3-netaddr
        state: installed

  roles:
    - checkmk.general.agent
```

---

# 💻 Installation of Checkmk agent

- 🛠️ Create required variables

```bash
mkdir host_vars
<EDITOR> host_vars/cmk-agent.yml
```

---

# 💻 Installation of Checkmk agent

```yaml
---
checkmk_agent_version: "2.5.0"
checkmk_agent_edition: "community"
checkmk_agent_server: localhost
checkmk_agent_server_validate_certs: "false"
checkmk_agent_site: "master"
checkmk_agent_user: "cmkadmin"
checkmk_agent_pass: "AnsibleW0rkshop2026!"
```

---

# 🧪 LAB 6: Install Checkmk agent

---

# 🧪 LAB 6: Install Checkmk agent

- ✅ Ensure Checkmk agent is installed
```bash
$ ansible-playbook -i hosts cmk_agent.yml

[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk agent] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-agent]

...

TASK [checkmk.general.agent : Update monitored services and labels on host.] *****************
ok: [cmk-agent]

PLAY RECAP ***********************************************************************************
cmk-agent : ok=19 changed=3 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

# 🔧 Checkmk server configuration

---

# 🗂️ Checkmk server configuration: Folders

---

# 🗂️ Checkmk configuration: Folders

- 🛠️ Create two folders - **/linux** and **/linux/checkmk**
- ➕ Add a **label** to folder linux and ensure **Checkmk agent** is used for monitoring

```bash
<EDITOR> host_vars/cmk-server.yml
```

---

# 🗂️ Checkmk configuration: Folders

```yaml
...
checkmk_folders:
  - path: /linux
    attributes:
      labels:
        os: almalinux
      tag_agent: "cmk-agent"
  - path: /linux/checkmk
```

**NOTE**
> Create the folder in the Web UI first and access the API to get syntax for attributes!

---

# 🗂️ Checkmk configuration: Folders

- 🛠️ Create two folders - **/linux** and **/linux/checkmk**
- ➕ Add a **label** to folder linux and ensure **Checkmk agent** is used for monitoring
- ➕ Append a new task to the existing tasks

```bash
<EDITOR> cmk_server.yml
```

---

<!-- _class: code-small -->
# 🗂️ Checkmk configuration: Folders

```yaml
...
    - name: Create folder
      checkmk.general.folder:
        name: "{{ item['name'] | default(omit) }}"
        path: "{{ item['path'] }}"
        attributes: "{{ item['attributes'] | default(omit) }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_folders }}"
```

---

<!-- _class: code-small -->
# 🗂️ Checkmk configuration: Folders

- ➕ Append a new handler
```yaml
...
    - name: Activate Checkmk changes
      checkmk.general.activation:
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
        force_foreign_changes: true
      run_once: true
```

---

# 🧪 LAB 7: Checkmk configuration: Folders

---

# 🧪 LAB 7: Checkmk configuration: Folders

- ✅ Ensure folder is created
```bash
ansible-playbook -i hosts cmk_server.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server : ok=19 changed=3 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

# 🖥️ Checkmk server configuration: Hosts

---

# 🖥️ Checkmk configuration: Hosts

- 🛠️ Create host **cmk-agent** in folder **/linux/checkmk** with IP address **127.0.0.1**
```bash
<EDITOR> host_vars/cmk-agent.yml
```

---

# 🖥️ Checkmk configuration: Hosts

```yaml
...
checkmk_host_settings:
  folder: /linux/checkmk
  attributes:
    ipaddress: "127.0.0.1"
```

**NOTE**
> Create the host in the Web UI first and access the API to get syntax for attributes!

---

# 🖥️ Checkmk configuration: Hosts

- 🛠️ Create host **cmk-agent** in folder **/linux/checkmk** with IP address **127.0.0.1**
- ➕ Add a new task
```bash
$ <EDITOR> cmk_agent.yml
```

---

<!-- _class: code-small -->
# 🖥️ Checkmk configuration: Hosts

```yaml
...
  tasks:
    - name: Create host
      checkmk.general.host:
        name: "{{ inventory_hostname }}"
        folder: "{{ checkmk_host_settings['folder'] | default(omit) }}"
        attributes: "{{ checkmk_host_settings['attributes'] | default(omit) }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_agent_pass }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_host_settings | dict2items }}"
```

---

<!-- _class: code-small -->
# 🖥️ Checkmk configuration: Hosts

- ➕ Append a new handler
```yaml
...
  handlers:
    - name: Activate Checkmk changes
      checkmk.general.activation:
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_agent_pass }}"
        validate_certs: false
        force_foreign_changes: true
      run_once: true
```

---

# 🧪 LAB 8: Checkmk configuration: Hosts

---

# 🧪 LAB 8: Checkmk configuration: Hosts

- ✅ Ensure host is created
```bash
$ ansible-playbook -i hosts cmk_agent.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk agent] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-agent]

...

PLAY RECAP ***********************************************************************************
cmk-agent : ok=21 changed=2 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

# 🔎 Checkmk server configuration: Service discovery

---

# 🔎 Checkmk configuration: Discovery

- 🔎 Run service discovery and add **unmonitored** services
- ➕ Append a task to the existing tasks
```bash
<EDITOR> cmk_agent.yml
```

---

<!-- _class: code-small -->
# 🔎 Checkmk configuration: Discovery

```yaml
...
    - name: Run service discovery
      checkmk.general.discovery:
        host_name: "{{ inventory_hostname }}"
        state: new
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_agent_pass }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
```

---

# 🧪 LAB 9: Checkmk configuration: Service discovery

---

# 🧪 LAB 9: Checkmk configuration: Discovery

- ✅ Ensure services are monitored
```bash
ansible-playbook -i hosts cmk_agent.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk agent] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-agent]

...

PLAY RECAP ***********************************************************************************
cmk-agent : ok=21 changed=2 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

<!-- _class: footnote-only -->
<!-- _backgroundImage: "url('assets/odp/100000010000024400000183D9D5699A.png')" -->
<!-- _backgroundSize: auto calc(100% - 3.5cm) -->
<!-- _backgroundPosition: center -->

<div class="corner-logos"></div>


---

# 👤 Checkmk server configuration: Users

---

# 👤 Checkmk configuration: Users

- 🛠️ Create user **cmkuser** with email address **cmkuser@localhost** and member of contact group **Everything** and role **Normal user**

```bash
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 👤 Checkmk configuration: Users

```yaml
...
checkmk_users:
  - name: cmkuser
    password: "AnsibleW0rkshop2026!"
    email: "cmkuser@localhost"
    fullname: "Checkmk user"
    auth_type: password
    contactgroups:
      - "all"
    roles:
      - "user"
```

---

# 👤 Checkmk configuration: Users

- 🛠️ Create user **cmkuser** with email address **cmkuser@localhost** and member of contact group **Everything** and role **Normal user**
- ➕ Add a new task after the existing tasks
```bash
<EDITOR> cmk_server.yml
```

---

<!-- _class: code-small -->
# 👤 Checkmk configuration: Users

```yaml
...
    - name: Create user
      checkmk.general.user:
        name: "{{ item['name'] }}"
        password: "{{ item['password'] | default(omit) }}"
        email: "{{ item['email'] | default(omit) }}"
        fullname: "{{ item['fullname'] | default(omit) }}"
        auth_type: "{{ item['auth_type'] }}"
        contactgroups: "{{ item['contactgroups'] | default(omit) }}"
        roles: "{{ item['roles'] | default(omit) }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_users }}"
```

---

# 🧪 LAB 10: Checkmk configuration: Users

---

# 🧪 LAB 10: Checkmk configuration: Users

- ✅ Ensure user is created
```bash
$ ansible-playbook -i hosts cmk_server.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server : ok=19 changed=3 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

# 🕒 Checkmk server configuration: Downtimes

---

# 🕒 Checkmk configuration: Downtimes

- 🛠️ Create downtime for **15 minutes**
```bash
<EDITOR> cmk_downtime.yml
```

---

<!-- _class: code-small -->
# 🕒 Checkmk configuration: Downtimes

```yaml
---

- name: Configure Checkmk downtime
  hosts: cmk-agent
  connection: local
  become: true

  tasks:
    - name: Create downtime
      checkmk.general.downtime:
        host_name: "{{ inventory_hostname }}"
        end_after:
          minutes: 15
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_agent_pass }}"
        validate_certs: false
```

---

# 🧪 LAB 11: Checkmk configuration: Downtimes

---

# 🧪 LAB 11: Checkmk configuration: Downtimes

- ✅ Ensure downtime is created
```bash
$ ansible-playbook -i hosts cmk_downtime.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Configure Checkmk downtime] ************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-agent]

...

PLAY RECAP ***********************************************************************************
cmk-agent : ok=3 changed=1 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0
```

---

# 📏 Checkmk server configuration: Rules

---

# 📏 Checkmk configuration: Rules

- 🛠️ Create a rule that sets the **Checkmk Agent** check to **warning** if the version is **different** from master and **critical** if **TLS** is **not activated**
```bash
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 📏 Checkmk configuration: Rules

```yaml
...
checkmk_rules:
  - ruleset: "checkgroup_parameters:agent_update"
    rule:
      properties:
        description: "Checkmk Agent version"
      location:
        folder: "/linux"
      value_raw: "{'agent_version': ('site', {}), 'agent_version_missmatch': 1, 'legacy_pull_mode': 2}"
```

**NOTE**
> Create the rule in the Web UI first and access the API to get syntax for rule values!

---

# 📏 Checkmk configuration: Rules

- 🛠️ Create a rule that sets the **Checkmk Agent** check to **warning** if the version is **different** from master and **critical** if **TLS** is **not activated**
- ➕ Add a task after the existing tasks
```bash
$ <EDITOR> cmk_server.yml
```

---

<!-- _class: code-small -->
# 📏 Checkmk configuration: Rules

```yaml
...
    - name: Create rule
      checkmk.general.rule:
        rule: "{{ item['rule'] }}"
        ruleset: "{{ item['ruleset'] }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_rules }}"
```

---

# 🧪 LAB 12: Checkmk configuration: Rules

---

# 🧪 LAB 12: Checkmk configuration: Rules

- ✅ Ensure the rule for agent monitoring is created
```bash
ansible-playbook -i hosts cmk_server.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server : ok=20 changed=3 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

# 🔐 Checkmk server configuration: Passwords

---

# 🔐 Checkmk configuration: Passwords

- 🛠️ Create a password for the site **authenticationtest.com**
```bash
<EDITOR> host_vars/cmk-server.yml
```

---

# 🔐 Checkmk configuration: Passwords

```yaml
...

checkmk_passwords:
  - name: authenticationtest_com
    title: authenticationtest.com
    password: pass
```

---

# 🔐 Checkmk configuration: Passwords

- 🛠️ Create a password for the site **authenticationtest.com**
- ➕ Add tasks **before the Create rule** task!
```bash
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 🔐 Checkmk configuration: Passwords

```yaml
...

    - name: Create password
      checkmk.general.password:
        name: "{{ item['name'] }}"
        title: "{{ item['title'] | default(omit) }}"
        password: "{{ item['password'] }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_passwords }}"
```

---

# 🔐 Checkmk configuration: Passwords

- 🛠️ Create a password for the site **authenticationtest.com**
- ➕ Add the rule to the existing rules
```bash
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 🔐 Checkmk configuration: Passwords

```yaml
...
checkmk_rules:
...
  - ruleset: "active_checks:httpv2"
    rule:
      properties:
        description: "authenticationtest.com"
      location:
        folder: "/"
      value_raw: >
        "{'endpoints': [{'service_name': {'prefix': 'auto', 'name': 'HTTPAuth'}, 'url': 'https://authenticationtest.com/HTTPAuth/'}],
        'standard_settings': {'connection': {'method': ('get', None), 'auth': ('user_auth', {'user': 'user', 'password': 
        ('cmk_postprocessed', 'stored_password', ('authenticationtest_com', ''))})}}}"
      conditions:
        host_name:
          match_on:
            - cmk-agent
          operator: one_of
```

**NOTE**
> Create the rule in the Web UI first and access the API to get syntax for rule values!

---

# 🧪 LAB 13: Checkmk configuration: Passwords

---

# 🧪 LAB 13: Checkmk configuration: Passwords

- ✅ Ensure the rule and password for web monitoring are created
```bash
$ ansible-playbook -i hosts cmk_server.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server : ok=21 changed=4 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

<!-- _class: footnote-only -->
<!-- _backgroundImage: "url('assets/odp/100000010000078000000500138259B5.png')" -->
<!-- _backgroundSize: auto calc(100% - 3.5cm) -->
<!-- _backgroundPosition: center -->

<div class="corner-logos"></div>

---

# 🔎 Fetch information from Checkmk with lookup plugins

---

# 🔎 Checkmk lookup

- 🖨️ Print **server version** of Checkmk server
- ➕ Add a task after the existing tasks
```bash
<EDITOR> cmk_server.yml
```

---

<!-- _class: code-small -->
# 🔎 Checkmk lookups

```yaml
...

    - name: Print Checkmk server version
      ansible.builtin.debug:
         msg: "Checkmk server version is {{ lookup('checkmk.general.version',
                              server_url='https://localhost',
                              site='master',
                              automation_user='cmkadmin',
                              automation_secret=checkmk_admin_pw,
                              validate_certs=false)
         }}"
```

---

# 🧪 LAB 14: Checkmk lookups

---

# 🧪 LAB 14: Checkmk lookups

- 🖨️ Print Checkmk server version
```bash
$ ansible-playbook -i hosts cmk_server.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]
...
TASK [Print Checkmk server version] **********************************************************
ok: [cmk-server] => {
  "msg": "Checkmk server version is 2.5.0.cre"
}

PLAY RECAP ***********************************************************************************
cmk-server : ok=22 changed=3 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

# 📚 Using Checkmk as Ansible inventory

---

# 📚 Checkmk as Ansible inventory

- ✅ Checkmk **should** be aware of all hosts
- 📚 Can be a good source for Ansible
- 🏷️ Most likely requires tag configuration

---

# 📚 Checkmk as Ansible inventory

- 🛠️ Create tags for **testing** and **production** hosts
```bash
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 📚 Checkmk as Ansible inventory

```yaml
...

checkmk_tag_groups:
  - name: lifecycle_environment
    title: Lifecycle Environment
    tags:
      - id: production
        title: Production
      - id: testing
        title: Testing
```

---

# 📚 Checkmk as Ansible inventory

- 🛠️ Create tags for **testing** and **production** hosts
- ➕ Add a task after the existing tasks
```bash
<EDITOR> host_vars/cmk-server.yml
```

---

<!-- _class: code-small -->
# 📚 Checkmk as Ansible inventory

```yaml
...
    - name: Create tag group
      checkmk.general.tag_group:
        name: "{{ item['name'] }}"
        title: "{{ item['title'] }}"
        tags: "{{ item['tags'] }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_tag_groups }}"
```

---

# 📚 Checkmk as Ansible inventory

- 🛠️ Create an inventory config for Checkmk
```bash
<EDITOR> checkmk.yml
```

**NOTE**
> Inventory plugin must end with checkmk.yml or checkmk.yaml!

---

# 📚 Checkmk as Ansible inventory

```yaml
---

plugin: checkmk.general.checkmk
server_url: https://localhost
site: master
automation_user: cmkadmin
automation_secret: "AnsibleW0rkshop2026!"
validate_certs: false
groupsources:
  - hosttags
  - sites
```

---

# 🧪 LAB 15: Checkmk as Ansible inventory

---

# 🧪 LAB 15: Checkmk as Ansible inventory

- ✅ Ensure tag groups are created
```bash
ansible-playbook -i hosts cmk_server.yml
```
```
[WARNING]: Collection checkmk.general does not support Ansible version 2.14.17

PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server : ok=23 changed=4 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

<!-- _class: code-small -->
# 🧪 LAB 15: Checkmk as Ansible inventory

- 🖨️ Print all hosts and groups
```bash
ansible-inventory -i checkmk.yml --graph
```
```bash
@all:
  |--@ungrouped:
  |--@tag_criticality_prod:
  |  |--cmk-agent
...
  |--@tag_lifecycle_environment_production:
  |  |--cmk-agent
  |--@tag_lifecycle_environment_testing:
...
  |--@tag_lifecycle_environment_production:
  |  |--cmk-agent
  |--@tag_lifecycle_environment_testing:
```

---

<!-- _class: code-small -->
# 🧪 LAB 15: Checkmk as Ansible inventory

- 🖨️ Print host details
```bash
ansible-inventory -i checkmk.yml --host cmk-agent
```
```json
{
    "checkmk_agent_edition": "cre",
    "checkmk_agent_pass": "AnsibleW0rkshop2026!",
    "checkmk_agent_server": "localhost",
    "checkmk_agent_server_validate_certs": "false",
    "checkmk_agent_site": "master",
    "checkmk_agent_user": "cmkadmin",
    "checkmk_agent_version": "2.5.0",
    "checkmk_host_settings": {
        "attributes": {
            "ipaddress": "127.0.0.1"
        },
        "folder": "/linux/checkmk"
    },
    "folder": "/linux/checkmk",
    "ipaddress": "127.0.0.1"
}
```

---

# 🚀 Demo: Self-healing with Checkmk, Ansible Automation Platform and rulebooks

---

# 💬 Feedback

---

# 💬 Feedback

- https://forms.gle/ki6KjQF9KS6BavmU8

![w:260](assets/odp/10000000000001C2000001C2D91A1DDE.png)

---

# 🙏 Thank you for participating

- DI (FH) René Koch
- Freelancer
- The Checkmk Conference #12, 18.06.2026
