# Material for Checkmk Conference 2026 Workshop

Workshop: Automate Checkmk with Ansible

- [Slides source](slides.md): Marp version of the 2025 workshop deck
- [LAB 1](lab1/etc/sudoers.d): Configure sudo
- [LAB 3](lab3/playbooks): Add target to Ansible inventory
- [LAB 4](lab4/playbooks): Install Checkmk server
- [LAB 5](lab5/playbooks): Secure Checkmk server
- [LAB 6](lab6/playbooks): Install Checkmk agent
- [LAB 7](lab7/playbooks): Checkmk configuration: Folders
- [LAB 8](lab8/playbooks): Checkmk configuration: Hosts
- [LAB 9](lab9/playbooks): Checkmk configuration: Service discovery
- [LAB 10](lab10/playbooks): Checkmk configuration: Users
- [LAB 11](lab11/playbooks): Checkmk configuration: Downtimes
- [LAB 12](lab12/playbooks): Checkmk configuration: Rules
- [LAB 13](lab13/playbooks): Checkmk configuration: Passwords
- [LAB 14](lab14/playbooks): Checkmk lookups
- [LAB 15](lab15/playbooks): Checkmk as Ansible inventory

## Build slides

```bash
npm install
npm run build
```

The Marp build creates `dist/index.html` and `dist/slides.pdf`.

The migrated deck is native Marp Markdown generated from the 2025 ODP source.
Some original embedded screenshots and logos are kept as individual image assets.

## Important links

- [Checkmk collection on Github](https://github.com/Checkmk/ansible-collection-checkmk.general)
- [Checkmk collection on Ansible Galaxy](https://galaxy.ansible.com/ui/repo/published/checkmk/general/)
- [Checkmk documentation](https://docs.checkmk.com/latest/en/)
- [Checkmk API documentation](https://docs.checkmk.com/latest/en/rest_api.html)
- [Checkmk download](https://checkmk.com/download)
- [Ansible documentation](https://docs.ansible.com/)
- [Red Hat Ansible Automation Platform](https://www.redhat.com/en/technologies/management/ansible)
- [Ansible Automation Platform documentation](https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.5)

## Contact

- René Koch
- [rkoch@rk-it.at](mailto:rkoch@rk-it.at)
- [LinkedIn Profile](https://www.linkedin.com/in/rk-it-at/)
