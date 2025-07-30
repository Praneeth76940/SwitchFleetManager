#  SwitchFleetManager – Automating Cisco IOS Upgrades with Jenkins + Ansible

##  About This Project

**SwitchFleetManager** is a practical automation project designed to make Cisco IOS upgrades easier and more reliable across multiple devices. As network engineers, we all know how time-consuming and error-prone manual upgrades can be — especially when you’re dealing with dozens (or hundreds) of switches and routers.

This project shows how to automate the entire upgrade process using **Ansible** and **Jenkins**, following real CI/CD workflows. It includes everything from backing up configs to setting boot variables and reloading devices (in a safe, simulated way using GNS3).

---

## 💼 Real-Life Scenario

In enterprise environments, IOS upgrades typically require:

- Long maintenance windows  
- Manual coordination between teams  
- Risk of human error  

With **SwitchFleetManager**, we can automate tasks like:

- Backing up existing configurations  
- Checking current IOS image  
- (Simulated) copying of new IOS  
- Setting the correct boot image  
- Saving the configuration  
- Reloading devices in a scheduled manner  

All these steps are run as a Jenkins job and can be triggered manually or automatically when a Git commit is made.

---

## 🔄 How It Works

Here's a quick breakdown of the workflow:

1. **Inventory File:**  
   A simple YAML file defines all router/switch info (IP, SSH creds).

2. **Ansible Playbook:**  
   Jenkins triggers the playbook to run upgrade tasks across all devices.

3. **Config Backup:**  
   Backs up the current running config and stores it with a timestamp.

4. **Flash Check (Simulated):**  
   Checks the current IOS using `dir flash:` (skipped in GNS3).

5. **Image Copy (Skipped):**  
   We simulate image transfer — no real IOS file copy in this lab.

6. **Boot Setting & Save:**  
   Sets the correct boot variable and saves the config.

7. **Reload (Optional):**  
   Performs a simulated reload — in production, this would be real.

---

## 🗂️ Folder Structure

```
SwitchFleetManager/
├── playbooks/
│   ├── upgrade_switch.yml
│   └── backup/
│       ├── R1_config_*.txt
│       └── R2_config_*.txt
├── inventory/
│   └── switches.yml
├── output_sample/
│   ├── IOS_Upgarde (Automation).png
│   ├── IOS_UpgardeJenkins.png
│   ├── Jenkins_Output.txt
│   └── Netboxproject.gns3
├── README.md
```

---

## 🚧 What’s Skipped in the Lab and Why

| Feature                  | Skipped? | Reason                                           |
|--------------------------|----------|--------------------------------------------------|
| Real IOS Copy via SCP    | ✅ Yes   | GNS3 doesn’t support real flash or IOS storage   |
| Actual Router Reload     | ✅ Yes   | Reload breaks SSH session in GNS3 environment    |
| Flash Check Output       | ✅ Yes   | Virtual flash doesn’t behave like real devices   |

---

## 🛠️ What You’d Do in Production

| Task               | How It’s Done in Real Networks                                  |
|--------------------|------------------------------------------------------------------|
| SCP Image Copy     | Use `copy scp://...` to transfer IOS images securely             |
| Flash Validation   | Run `dir flash:` and compare the MD5 checksum                   |
| Coordinated Reload | Use `reload at HH:MM` to align with change windows              |
| Jenkins Alerts     | Use Jenkins post-build actions to send Slack/email notifications|

---

## 📁 Output Files

The playbook automatically generates config backups like:

```
R1_config_2025-07-29_19-16-26.txt
```

These are stored under `playbooks/backup/` and pushed to Git for version control. You can track exactly what was running before any upgrade.

---

## 🧠 Final Thoughts

- This project is built in GNS3, but it’s fully adaptable to real environments.
- It’s a great way to bring **CI/CD practices into network engineering (NetDevOps)**.
- Automating IOS upgrades can save huge amounts of time and avoid costly mistakes.

---

### 🙌 Built with 💻 by **Praneeth Reddy**

> Feel free to fork, clone, or improve this project. Let's make network automation smarter, faster, and safer!
