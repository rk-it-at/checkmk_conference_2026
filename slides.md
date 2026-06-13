---
marp: true
title: Automate Checkmk with Ansible
theme: rk-it
size: 16:9
paginate: true
footer: Checkmk Conference #12, 18.06.2026
---

<!-- _class: conference-cover -->
# Automate Checkmk with Ansible

<div class="conference-cover-speaker">
  <img src="assets/conference-cover-speaker.png" alt="René Koch">
  <div>
    <strong>René Koch</strong>
    <span>DI (FH) René Koch</span>
  </div>
</div>

---

<!-- _class: right-bg-author -->
# 👋 About me

- 👤 René Koch
- 💼 Self-employed consultant for:
  - Red Hat Ansible (Automation Platform)
  - Red Hat Enterprise Linux
  - Red Hat Satellite
  - Red Hat Identity Management (IPA)
- 📈 Experienced monitoring user (Nagios, Icinga,
  Checkmk)

---

<!-- _class: right-bg-author -->
# 👋 About me

- 👤 René Koch
  - rkoch@rk-it.at
  - +43 660 / 464 0 464
  - https://www.linkedin.com/in/rk-it-at
  - https://github.com/rk-it-at
  - https://github.com/scrat14

---
<!-- _class: conference-divider -->

# 👥 Please introduce yourself

---

# 🕘 Timetable

- 🧑‍🏫 09:00 – 10:30: Workshop
- ☕ 10:30 – 10:45: Break
- 🧑‍🏫 10:45 – 12:30: Workshop
- 🍽️ 12:30 – 13:45: Lunch break
- 🧑‍🏫 13:45 – 15:30: Workshop
- ☕ 15:30 – 15:45: Break
- 🧑‍🏫 15:45 – 17:15: Workshop

---

# 🧭 Table of contents

- 🧪 Prepare the LAB environment
- 🖥️ Installation of Checkmk server
- 💻 Installation of Checkmk agents
- 🔧 Checkmk server configuration
- 🔎 Fetch information with lookup plugins
- 📚 Using Checkmk as Ansible inventory
- ♻️ Self-healing Checkmk agent with Event-Driven Ansible

---
<!-- _class: conference-divider -->

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

- ✅ Solutions for all labs are available in the GitHub repository:
  https://github.com/rk-it-at/checkmk_conference_2026
- 📄 Workshop slides as PDF:
  https://rk-it-at.github.io/checkmk_conference_2026/slides.pdf
- 🌐 Workshop slides on GitHub Pages:
  https://rk-it-at.github.io/checkmk_conference_2026/

---

# 🧪 Preparing the LAB environment

- 🖥️ Available VMs (if you do not have your own)
- 🐧 Operating system: AlmaLinux 10.2
- ✏️ Editors: Vim and Nano
- 🧰 Pre-installed tools: Git, Python and pip
- 🔐 Passwords: AnsibleW0rkshop2026!

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

---

<!-- _class: code-small -->
# 🧪 Preparing the LAB environment

| Hostname | IP Address | Username | Participant |
| --- | --- | --- | --- |
| ansible-ws-6 |  | ansible |  |
| ansible-ws-7 |  | ansible |  |
| ansible-ws-8 |  | ansible |  |
| ansible-ws-9 |  | ansible |  |
| ansible-ws-10 |  | ansible |  |
| ansible-ws-11 |  | ansible |  |

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
> Replace <EDITOR> with vi, nano or your favorite editor.
> Replace <USERNAME> with your system user.

---
<!-- _class: conference-divider -->

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
> Replace <EDITOR> with vi, nano or your favorite editor.
> Replace <USERNAME> with your system user.

---
<!-- _class: conference-divider -->

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
sudo subscription-manager repos --enable codeready-builder-for-rhel-10-$(arch)-rpms
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-10.noarch.rpm
sudo dnf install ansible-core
```

---

# ⚙️ Install Ansible – RHEL derivatives

- ⚙️ Install Ansible on AlmaLinux or Rocky Linux 10

```bash
sudo dnf config-manager --set-enabled crb
sudo dnf install epel-release
sudo dnf install ansible-core
```

---

# ⚙️ Install Ansible – openSUSE

- ⚙️ Install Ansible on openSUSE

```bash
sudo zypper install ansible
```

---

# ⚙️ Install Ansible – Ubuntu

- ⚙️ Install Ansible on Ubuntu 24.04+

```bash
sudo apt update
sudo apt install ansible
```

---

# ⚙️ Install Ansible – pip

- ⚙️ Install Ansible with pip (or consider uv as an alternative)

```bash
pip install ansible
```

**NOTE**
> Use a virtual environment outside the workshop!

---
<!-- _class: conference-divider -->

# 🧪 LAB 2: Install Ansible

---

# 🧪 LAB 2: Install Ansible

- ✅ Ensure Ansible is installed

```bash
ansible --version
```
```
ansible [core 2.21.0]
```

**NOTE**
> The available Ansible version depends on your operating system.
> On RHEL 10, use at least version 2.16.18 due to a bug involving PQC GPG keys and the `rpm_key` Ansible module!
> On RHEL 8, use at most version 2.16.18!

---
<!-- _class: conference-divider -->

# 📋 Prepare Ansible inventory

---

# 📋 Prepare Ansible inventory

- 🛠️ Create a folder for your Ansible code
```bash
mkdir playbooks
cd playbooks
<EDITOR> hosts
cmk-server ansible_connection=local
cmk-agent ansible_connection=local
```

**NOTE**
> Replace <EDITOR> with vi, nano or your favorite editor.

---

<!-- _class: code-small -->
# 🔌 Test connection to target hosts

- ✅ Make sure your target hosts are reachable
```yaml
ansible all -i hosts -m ping
cmk-server | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
    }
cmk-agent | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.12"
    },
    "changed": false,
    "ping": "pong"
    }
```

> Ansible does not send an ICMP ping to test the connection; it tries to log in to the target machine.

---
<!-- _class: conference-divider -->

# 🧪 LAB 3: Add target to Ansible inventory

---

<!-- _class: code-small -->
# 🧪 LAB 3: Add target to Ansible inventory

- ✅ Make sure your target hosts are reachable
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
<!-- _class: conference-divider -->

# 📦 Install Checkmk Collection

---

# 📦 Install Checkmk Collection

- 📦 Collections provide simplified and consistent content delivery
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
<!-- _class: conference-divider -->

# 🖥️ Installation of Checkmk server

---

# 🖥️ Installation of Checkmk server

- 📦 Install Checkmk with the provided server role
- 💡 Consider writing your own role to fit your Ansible environment and coding guidelines!
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

checkmk_server_version: "2.5.0p5"
checkmk_server_edition: "community"
checkmk_admin_pw: "AnsibleW0rkshop2026!"
checkmk_server_sites:
  - name: "master"
    version: "{{ checkmk_server_version }}"
    update_conflict_resolution: "abort"
    edition: "{{ checkmk_server_edition }}"
    admin_pw: "{{ checkmk_admin_pw }}"
    state: started
```

---
<!-- _class: conference-divider -->

# 🧪 LAB 4: Install Checkmk server

---

# 🧪 LAB 4: Install Checkmk server

- ✅ Ensure Checkmk server is installed
```bash
ansible-playbook -i hosts cmk_server.yml

PLAY [Install Checkmk server] **************************************************


TASK [checkmk.general.server : Validating arguments against arg spec 'main' - Install and manage Checkmk servers] ******************
ok: [cmk-server]

...

RUNNING HANDLER [checkmk.general.server : Start httpd] ****************************************
ok: [cmk-server]

PLAY RECAP *****************************************************************
cmk-server                 : ok=23   changed=11   unreachable=0    failed=0    skipped=25   rescued=0    ignored=0
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
<!-- _class: conference-divider -->

# 🧪 LAB 5: Secure Checkmk server

---

# 🧪 LAB 5: Secure Checkmk server

- ✅ Ensure Checkmk server is listening to HTTPS

```bash
ansible-playbook -i hosts cmk_server.yml

...

PLAY RECAP *******************************************************************************************************
cmk-server                 : ok=20   changed=3    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
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
<!-- _class: conference-divider -->

# 💻 Installation of Checkmk agent

---

# 💻 Installation of Checkmk agent

- 📦 Install the Checkmk agent with the provided agent role
- 💡 Consider writing your own role to fit your Ansible environment and coding guidelines!
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

  roles:
    - checkmk.general.agent
```

---

# 💻 Installation of Checkmk agent

- 🛠️ Create required variables

```bash
<EDITOR> host_vars/cmk-agent.yml
```

---

# 💻 Installation of Checkmk agent

```yaml
---

checkmk_agent_version: "2.5.0p5"
checkmk_agent_edition: "community"
checkmk_agent_server: localhost
checkmk_agent_server_validate_certs: "false"
checkmk_agent_site: "master"
checkmk_agent_user: "cmkadmin"
checkmk_agent_pass: "AnsibleW0rkshop2026!"
```

---
<!-- _class: conference-divider -->

# 🧪 LAB 6: Install Checkmk agent

---

# 🧪 LAB 6: Install Checkmk agent

- ✅ Ensure Checkmk agent is installed
```bash
$ ansible-playbook -i hosts cmk_agent.yml

PLAY [Install Checkmk agent] ****************************************************************

...

PLAY RECAP ***********************************************************************************
cmk-agent : ok=19 changed=4 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---
<!-- _class: conference-divider -->

# 🔧 Checkmk server configuration

---
<!-- _class: conference-divider -->

# 🗂️ Checkmk server configuration: Folders

---

# 🗂️ Checkmk configuration: Folders

- 🛠️ Create two folders: **/linux** and **/linux/checkmk**
- ➕ Add a **label** to `/linux` and ensure the **Checkmk agent** is used for monitoring

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
> Create the folder in the web UI first and use the API to determine the attribute syntax!

---

# 🗂️ Checkmk configuration: Folders

- 🛠️ Create two folders: **/linux** and **/linux/checkmk**
- ➕ Add a **label** to `/linux` and ensure the **Checkmk agent** is used for monitoring
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
<!-- _class: conference-divider -->

# 🧪 LAB 7: Checkmk configuration: Folders

---

# 🧪 LAB 7: Checkmk configuration: Folders

- ✅ Ensure both folders are created
```bash
ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=21   changed=3    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0 
```

---
<!-- _class: conference-divider -->

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
> Create the host in the web UI first and use the API to determine the attribute syntax!

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
<!-- _class: conference-divider -->

# 🧪 LAB 8: Checkmk configuration: Hosts

---

# 🧪 LAB 8: Checkmk configuration: Hosts

- ✅ Ensure host is created
```bash
$ ansible-playbook -i hosts cmk_agent.yml
```
```
PLAY [Install Checkmk agent] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-agent]

...

PLAY RECAP ***********************************************************************************
cmk-agent : ok=21 changed=4 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---
<!-- _class: conference-divider -->

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
<!-- _class: conference-divider -->

# 🧪 LAB 9: Checkmk configuration: Service discovery

---

# 🧪 LAB 9: Checkmk configuration: Discovery

- ✅ Ensure services are monitored
```bash
ansible-playbook -i hosts cmk_agent.yml
```
```
PLAY [Install Checkmk agent] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-agent]

...

PLAY RECAP ***********************************************************************************
cmk-agent : ok=22 changed=4 unreachable=0 failed=0 skipped=25 rescued=0 ignored=0
```

---

<!-- _class: footnote-only -->
<!-- _backgroundImage: "url('assets/odp/100000010000024400000183D9D5699A.png')" -->
<!-- _backgroundSize: auto calc(100% - 3.5cm) -->
<!-- _backgroundPosition: center -->

<div class="corner-logos"></div>


---
<!-- _class: conference-divider -->

# 👤 Checkmk server configuration: Users

---

# 👤 Checkmk configuration: Users

- 🛠️ Create user **cmkuser** with email address **cmkuser@localhost**, membership in contact group **Everything** and role **Normal user**

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

- 🛠️ Create user **cmkuser** with email address **cmkuser@localhost**, membership in contact group **Everything** and role **Normal user**
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
<!-- _class: conference-divider -->

# 🧪 LAB 10: Checkmk configuration: Users

---

# 🧪 LAB 10: Checkmk configuration: Users

- ✅ Ensure user is created
```bash
$ ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=22   changed=3    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
```

---
<!-- _class: conference-divider -->

# 🕒 Checkmk server configuration: Downtimes

---

# 🕒 Checkmk configuration: Downtimes

- 🛠️ Create a **15-minute** downtime
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
<!-- _class: conference-divider -->

# 🧪 LAB 11: Checkmk configuration: Downtimes

---

# 🧪 LAB 11: Checkmk configuration: Downtimes

- ✅ Ensure the downtime is created
```bash
$ ansible-playbook -i hosts cmk_downtime.yml
```
```
PLAY [Configure Checkmk downtime] ******************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************
ok: [cmk-agent]

TASK [Create downtime] *****************************************************************************************************************************************
changed: [cmk-agent]

PLAY RECAP *****************************************************************************************************************************************************
cmk-agent                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

---
<!-- _class: conference-divider -->

# 📏 Checkmk server configuration: Rules

---

# 📏 Checkmk configuration: Rules

- 🛠️ Create a rule that sets the **Checkmk Agent** service to **warning** when its version differs from the site and to **critical** when **TLS** is disabled
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
> Create the rule in the web UI first and use the API to determine the rule value syntax!

---

# 📏 Checkmk configuration: Rules

- 🛠️ Create a rule that sets the **Checkmk Agent** service to **warning** when its version differs from the site and to **critical** when **TLS** is disabled
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
<!-- _class: conference-divider -->

# 🧪 LAB 12: Checkmk configuration: Rules

---

# 🧪 LAB 12: Checkmk configuration: Rules

- ✅ Ensure the rule for agent monitoring is created
```bash
ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=23   changed=3    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0 
```

---
<!-- _class: conference-divider -->

# 🔐 Checkmk server configuration: Passwords

---

# 🔐 Checkmk configuration: Passwords

- 🛠️ Create a password for **authenticationtest.com**
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

- 🛠️ Create a password for **authenticationtest.com**
- ➕ Add the task **before the Create rule** task!
```bash
<EDITOR> cmk_server.yml
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

- 🛠️ Create a password for **authenticationtest.com**
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
      value_raw: "{'endpoints': [{'service_name': {'prefix': 'auto', 'name': 'HTTPAuth'}, 'url': 'https://authenticationtest.com/HTTPAuth/'}],
        'standard_settings': {'connection': {'method': ('get', None), 'auth': ('user_auth', {'user': 'user', 'password': 
        ('cmk_postprocessed', 'stored_password', ('authenticationtest_com', ''))})}}}"
      conditions:
        host_name:
          match_on:
            - cmk-agent
          operator: one_of
```

**NOTE**
> Create the rule in the web UI first and use the API to determine the rule value syntax!

---
<!-- _class: conference-divider -->

# 🧪 LAB 13: Checkmk configuration: Passwords

---

# 🧪 LAB 13: Checkmk configuration: Passwords

- ✅ Ensure the rule and password for web monitoring are created
```bash
$ ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=24   changed=4    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
```

---

<!-- _class: footnote-only -->
<!-- _backgroundImage: "url('assets/odp/100000010000078000000500138259B5.png')" -->
<!-- _backgroundSize: auto calc(100% - 3.5cm) -->
<!-- _backgroundPosition: center -->

<div class="corner-logos"></div>

---
<!-- _class: conference-divider -->

# 🔎 Fetch information from Checkmk with lookup plugins

---

# 🔎 Checkmk lookup

- 🖨️ Print the **Checkmk server version**
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
<!-- _class: conference-divider -->

# 🧪 LAB 14: Checkmk lookups

---

# 🧪 LAB 14: Checkmk lookups

- 🖨️ Print Checkmk server version
```bash
$ ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

TASK [Print Checkmk server version] **********************************************************
ok: [cmk-server] => {
  "msg": "Checkmk server version is 2.5.0p5.community"
}

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=25   changed=4    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
```

---
<!-- _class: conference-divider -->

# 📚 Using Checkmk as Ansible inventory

---

# 📚 Checkmk as Ansible inventory

- ✅ Checkmk **should** know about all hosts
- 📚 Checkmk can be a good inventory source for Ansible
- 🏷️ Grouping hosts usually requires tag configuration

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
<EDITOR> cmk_server.yml
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

- 🛠️ Create an inventory configuration for Checkmk
```bash
<EDITOR> checkmk.yml
```

**NOTE**
> The inventory plugin filename must end in `checkmk.yml` or `checkmk.yaml`!

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

# 🔮 Preview: Upcoming inventory plugin changes

- 📁 Restrict hosts by folder recursively or non-recursively
- 🏷️ Exclude hosts based on specific tags
- 🔡 Convert hostnames to lowercase
- 🌐 Append domains to hostnames

- 🚧 In development:
  https://github.com/Checkmk/ansible-collection-checkmk.general/blob/feature/inventory-host-filtering/plugins/inventory/checkmk.py

---
<!-- _class: conference-divider -->

# 🧪 LAB 15: Checkmk as Ansible inventory

---

# 🧪 LAB 15: Checkmk as Ansible inventory

- ✅ Ensure tag groups are created
```bash
ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=26   changed=5    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
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
    "checkmk_agent_version": "2.5.0p5",
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
<!-- _class: conference-divider -->

# 🧰 More checkmk.general modules

---

# 🧰 More checkmk.general modules

- ⏱️ Not covered today due to time constraints
- 👥 `checkmk.general.contact_group`
  - Manage contact groups
- 🖥️ `checkmk.general.host_group`
  - Manage host groups
- 🧩 `checkmk.general.service_group`
  - Manage service groups
- 🗓️ `checkmk.general.timeperiod`
  - Manage time periods

---

# 🧰 More checkmk.general modules

- 🥖 `checkmk.general.bakery`
  - Bake and sign Checkmk agents
- 🏗️ `checkmk.general.site`
  - Create, update and manage Checkmk sites
- 🔄 `checkmk.general.dcd`
  - Manage dynamic host management
- 👥 `checkmk.general.ldap`
  - Manage LDAP connections
- 📚 Module documentation:
  https://galaxy.ansible.com/ui/repo/published/checkmk/general/content/

---
<!-- _class: conference-divider -->

# 🧪 (Bonus) LAB 16: Self-healing Checkmk agent

---

# 🧪 LAB 16: Self-healing Checkmk agent

- 🎯 Goal: automatically restart a failed Checkmk agent
- 🔔 Checkmk sends service alerts to an EDA webhook
- 📖 A rulebook matches a critical `Check_MK` service
- ▶️ `run_playbook` starts the remediation playbook
- ♻️ The playbook restarts `check-mk-agent.socket`

---

# 🧪 LAB 16: Install Event-Driven Ansible

- 📦 Install the rulebook runner

```bash
pip install ansible-rulebook
```

- ☕ Install OpenJDK

```bash
sudo dnf install java-latest-openjdk
```

- 📚 Install the EDA collection

```bash
ansible-galaxy collection install ansible.eda
```

---

# 🧪 LAB 16: Install Event-Driven Ansible

- 🔎 Verify the installation

```bash
ansible-rulebook --version
```
```
ansible-rulebook [1.3.0]
  Executable location = /home/ansible/.local/bin/ansible-rulebook
  Drools_jpy version = 0.4.0
  Java home = /usr/lib/jvm/java-latest-openjdk
  Java version = 26.0.1
  Ansible core version = 2.21.0
  Python version = 3.12.13
  Python executable = /usr/bin/python3
  Platform = Linux-6.12.0-211.20.1.el10_2.x86_64-x86_64-with-glibc2.39
```

---

# 🧪 LAB 16: Install the notification script

- 🛠️ Create `notify_eda.sh`
```bash
<EDITOR> notify_eda.sh
```

---

# 🧪 LAB 16: Install the notification script

```bash
#!/usr/bin/env bash
# Event-Driven Ansible

URL="${NOTIFY_PARAMETER_1}"

JSON=$(cat <<EOF
{
  "hostname": "${NOTIFY_HOSTNAME}",
  "hostoutput": "${NOTIFY_HOSTOUTPUT}",
  "hoststate": "${NOTIFY_HOSTSTATE}",
  "servicename": "${NOTIFY_SERVICEDESC}",
  "serviceoutput": "${NOTIFY_SERVICEOUTPUT}",
  "servicestate": "${NOTIFY_SERVICESTATE}",
  "date": "${NOTIFY_SHORTDATETIME}",
  "type": "${NOTIFY_NOTIFICATIONTYPE}",
  "what": "${NOTIFY_WHAT}"
}
EOF
)

curl -X POST -H "Content-Type: application/json" -d "${JSON}" ${URL}
exit $?
```

---

# 🧪 LAB 16: Install the notification script

- 🛠️ Deploy notification script to Checkmk server
```bash
<EDITOR> cmk_server.yml
```

---

# 🧪 LAB 16: Install the notification script

```yaml
...
    - name: Install EDA notification script
      ansible.builtin.copy:
        src: notify_eda.sh
        dest: /omd/sites/master/local/share/check_mk/notifications/notify_eda.sh
        owner: master
        group: master
        mode: "0755"
      notify: Restart Checkmk site

  handlers:
    - name: Restart Checkmk site
      ansible.builtin.command: omd restart master
...
```

---

# 🧪 LAB 16: Install the notification script

- ▶️ Ensure the notification script is installed

```bash
ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=27   changed=7    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
```

---

# 🧪 LAB 16: Define the notification rule

- 🛠️ Create the notification rule **Forward alerts to Event-Driven Ansible** and send notifications to **localhost:5050**
```bash
<EDITOR> host_vars/cmk-server.yml
```
---

# 🧪 LAB 16: Define the notification rule

<!-- _class: code-small -->
```yaml
checkmk_notifications:
  - rule_properties:
      description: "Forward alerts to Event-Driven Ansible"
    notification_method:
      notify_plugin:
        option: "create_notification_with_custom_parameters"
        plugin_params:
          plugin_name: "notify_eda.sh"
          params:
            - "http://127.0.0.1:5050"
      notification_bulking:
        state: "disabled"
    contact_selection:
      members_of_contact_groups:
        value:
          - all
        state: "enabled"
```

---

# 🧪 LAB 16: Define the notification rule

- 🛠️ Create the notification rule **Forward alerts to Event-Driven Ansible** and send notifications to **localhost:5050**
```bash
<EDITOR> cmk_server.yml
```
---

# 🧪 LAB 16: Create the notification rule

```yaml
...
    - name: Create notification rule
      checkmk.general.notification:
        rule_config: "{{ item }}"
        state: present
        server_url: "https://localhost"
        site: "master"
        automation_user: "cmkadmin"
        automation_secret: "{{ checkmk_admin_pw }}"
        validate_certs: false
      notify: "Activate Checkmk changes"
      loop: "{{ checkmk_notifications }}"
```

---

# 🧪 LAB 16: Create the notification rule

- ✅ Ensure the notification rule is created

```bash
ansible-playbook -i hosts cmk_server.yml
```
```
PLAY [Install Checkmk server] ****************************************************************

TASK [Gathering Facts] ***********************************************************************
ok: [cmk-server]

...

PLAY RECAP ***********************************************************************************
cmk-server                 : ok=28   changed=6    unreachable=0    failed=0    skipped=29   rescued=0    ignored=0
```

---

# 🧪 LAB 16: Create the remediation playbook

- 🛠️ Create a playbook that **restarts the Checkmk agent**

```bash
<EDITOR> restart_checkmk_agent.yml
```
---

# 🧪 LAB 16: Create the remediation playbook

```yaml
---

- name: Restart Checkmk agent
  hosts: "{{ target_host | default('cmk-agent') }}"
  become: true

  tasks:
    - name: Restart Checkmk agent socket
      ansible.builtin.systemd_service:
        name: check-mk-agent.socket
        state: restarted
```

---

# 🧪 LAB 16: Create the rulebook

- 🛠️ Create a rulebook that **listens on port 5050** and checks for a **failed Checkmk agent**

```bash
mkdir rulebooks
<EDITOR> rulebooks/restart_checkmk_agent.yml
```

---

# 🧪 LAB 16: Create the rulebook

```yaml
---
- name: Restart Checkmk agent
  hosts: all
  gather_facts: false

  sources:
    - name: Listen for Checkmk notifications
      ansible.eda.webhook:
        host: 127.0.0.1
        port: 5050

  rules:
    - name: Restart a failed Checkmk agent
      condition: >-
        event.payload.servicename == "Check_MK" and
        event.payload.servicestate == "CRITICAL"
      action:
        run_playbook:
          name: restart_checkmk_agent.yml
          extra_vars:
            target_host: "{{ event.payload.hostname }}"
```

---

# 🧪 LAB 16: Run the rulebook

- ▶️ Start the rulebook in the first SSH session

```bash
ansible-rulebook -i hosts -r rulebooks/restart_checkmk_agent.yml --verbose
```
```
2026-06-11 17:35:09,830 - ansible_rulebook.app - INFO - Starting sources
2026-06-11 17:35:09,830 - ansible_rulebook.app - INFO - Starting rules
...
2026-06-11 17:35:10,506 - ansible_rulebook.rule_set_runner - INFO - Waiting for actions on events from Restart Checkmk agent
2026-06-11 17:35:10,506 - ansible_rulebook.rule_set_runner - INFO - Waiting for events, ruleset: Restart Checkmk agent
```

- 👂 Keep the rulebook running while it waits on port `5050`!

---

# 🧪 LAB 16: Trigger self-healing

- 🛑 Stop the Checkmk agent in the second SSH session

```bash
sudo systemctl stop check-mk-agent.socket
```

- ⏳ Wait for the `Check_MK` service to become critical
- 👀 Watch the rulebook run `restart_checkmk_agent.yml`
- ✅ Confirm that the socket was restarted

---

# 🧪 LAB 16: Trigger self-healing

```
...
2026-06-11 17:42:55,255 - ansible_rulebook.rule_set_runner - INFO - Waiting for events, ruleset: Restart Checkmk agent
2026-06-11 17:43:02,511 - aiohttp.access - INFO - 127.0.0.1 [11/Jun/2026:17:43:02 +0200] "POST / HTTP/1.1" 200 152 "-" "curl/8.12.1"

PLAY [Restart Checkmk agent] ***************************************************

TASK [Gathering Facts] *********************************************************
ok: [cmk-agent]

TASK [Restart Checkmk agent socket] ********************************************
changed: [cmk-agent]

PLAY RECAP *********************************************************************
cmk-agent                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

---
<!-- _class: conference-divider -->

# 💬 Feedback

---

# 💬 Feedback

- 🔗 https://forms.gle/Xsrwr6GqxRHTe32Z7

![w:260](assets/odp/10000000000001C2000001C2D91A1DDE.png)

---

# 🙏 Thank you for participating

- 👤 DI (FH) René Koch
- 💼 Freelancer
- 📅 The Checkmk Conference #12, 18.06.2026
