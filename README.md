# Ansible Proxmox HAOS Deployer 🏠🚀

![Ansible](https://img.shields.io/badge/Ansible-2.15+-black?style=for-the-badge&logo=ansible)
![Proxmox](https://img.shields.io/badge/Proxmox-VE-orange?style=for-the-badge&logo=proxmox)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

This Ansible project automates the full deployment of **Home Assistant OS (HAOS)** on a **Proxmox VE** node. It follows DevOps best practices, ensuring an idempotent and secure setup.

---

## ✨ Features

* **Automated Image Handling:** Downloads and extracts the latest HAOS image directly on the Proxmox node.
* **Idempotent:** Smart checks ensure the VM and disks are only created if they don't already exist.
* **Secure:** Sensitive API credentials are encrypted using **Ansible Vault**.
* **Highly Customizable:** Easily adjust CPU, RAM, and HAOS versions via variables.
* **Clean Output:** Optimized with handlers and formatted for clear terminal feedback.

---

## 🛠 Prerequisites

Before starting, ensure you have the following:

1.  **Ansible** (>= 2.15) installed on your local machine.
2.  **Proxmox API Token:** A token with `PVEVMAdmin` permissions.
3.  **Ansible Collection:** `ansible-galaxy collection install community.general`

---

## 🚀 Setup Instructions

### 1. Configure Inventory
Edit `inventories/homelab/hosts.yml` and replace the IP address and SSH user with your Proxmox node details.

### 2. Setup Secrets (Ansible Vault)
We use Ansible Vault to keep your API tokens secure. Create the encrypted vault file:
`ansible-vault create inventories/homelab/group_vars/proxmox_servers/vault.yml`

Inside the vault, add your secret:
`vault_proxmox_api_token_secret: "YOUR_PROXMOX_API_TOKEN_SECRET"`

### 3. Adjust Variables (Optional)
Customize your VM specifications (CPU, RAM, HAOS Version) in:
`inventories/homelab/group_vars/proxmox_servers/vars.yml`

---

## 🏗 Running the Deployment

Execute the playbook and provide your vault password when prompted:

`ansible-playbook playbooks/deploy_haos.yml --ask-vault-pass`

### What happens under the hood?

1.  **Environment Check:** Ensures `xz-utils` is present on the Proxmox host.
2.  **Image Prep:** Downloads and unpacks the HAOS `.qcow2` image.
3.  **VM Provisioning:** Creates the VM skeleton (UEFI, q35) if missing.
4.  **Disk Import:** Imports the image and attaches it to the VM.
5.  **Finalization:** Configures the boot order and starts the VM automatically via handlers.

---

## 🔒 Security Note

The `vault.yml` is encrypted, but it is best practice to **never** push unencrypted secrets to a public repository.
> **Important:** Always verify that your `.gitignore` is active and excludes any temporary unencrypted files.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.