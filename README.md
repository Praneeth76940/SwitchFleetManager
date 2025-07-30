SwitchFleetManager – Cisco IOS Upgrade Automation with Jenkins + Ansible
Project Overview
SwitchFleetManager is a real-time network automation project that simulates Cisco IOS upgrades on multiple routers using Ansible and Jenkins. It is designed for network engineers who want to automate routine tasks like configuration backup, OS upgrade, and device reboot using a standardized playbook.

This project also integrates with Jenkins CI/CD to automatically execute the upgrade process as part of a build job triggered by Git commits or manual runs.

Real-Time Use Case

In enterprise networks, upgrading IOS on hundreds of routers/switches is:

Time-consuming

Error-prone

Requires strict coordination

This tool helps automate:

Backing up existing configurations

Checking current image in flash

Copying the new IOS image (simulated here)

Setting boot variables

Saving configurations

Reloading devices (soft reboot)

⚙How It Works (Flow)
Inventory: YAML-based file listing routers (e.g. R1, R2) with SSH credentials.

Playbook Execution: Jenkins runs the Ansible playbook on all routers.

Backup: Saves current config files (R1_config_xx.txt, R2_config_xx.txt)

Flash Check: Simulates checking current image using dir flash:

Image Copy: Skips real image transfer (in GNS3, flash is virtual)

Boot Config: Sets the new IOS image as the boot variable

Save & Reload: Saves configuration and reloads router (timeout-simulated)

File Structure
Copy
Edit
SwitchFleetManager/
├── playbooks/
│   ├── upgrade_switch.yml
│   └── backup/
│       ├── R1_config_2025-07-29_19-16-26.txt
│       └── R2_config_2025-07-29_19-16-26.txt
├── inventory/
│   └── switches.yml
├── README.md
🚫 What We Skipped in Lab (and Why)
Feature	Skipped?	Reason
Actual IOS Copy via SCP	✅ Yes	GNS3 devices don’t support real flash/IOS transfer
Real Device Reload	✅ Yes	Router reload leads to session loss in GNS3 and halts playbook
Flash Check Output	✅ Yes	dir flash: fails due to GNS3's virtual flash setup

✅ How to Handle in Real Environment
Task	Real-world Handling
SCP Image Transfer	Use copy scp://... in production where routers support SCP
Flash Validation	Check image with dir flash: and compare MD5 hash
Reload Coordination	Schedule reload via reload at HH:MM or during maintenance window
Jenkins Integration	Use credential management and post-build actions to send Slack/email notifications

📤 Output Files Committed to Git
Config backups are auto-generated and renamed for Git (e.g. R1_config_2025-07-29_19-16-26.txt)

These help verify what config was in place before upgrade

Stored under playbooks/backup/ and pushed via Git CLI

🧠 Final Notes
Project is simulation-ready, but 100% real-world extendable

Follows CI/CD workflow for NetOps teams

Helps avoid human errors and speeds up fleet-wide upgrades


Project Made by Praneeth Reddy
