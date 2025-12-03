# 🚀 VPS Infrastructure as Code

A complete, battle-tested Ansible collection to deploy a modern, secure, and lightweight self-hosted infrastructure.

## 🔥 Key Highlights

### ⚡ Ultra-Light Matrix & VoIP Stack (Tuwunel)
Forget heavy Synapse. We run a highly optimized communication stack:
- **Conduit (Tuwunel):** The Rust-based Matrix homeserver. Blazing fast, super low memory footprint.
- **LiveKit:** High-performance WebRTC SFU for crystal clear audio/video calls.
- **Element Call Integration:** Full support for **Element X** native calls via a dedicated JWT-provider service.
> **Result:** A private Discord/Telegram alternative with video calls that runs on a $5 VPS.

### 🔐 Zero-Touch Identity (SSO)
A fully automated authentication chain protecting your services:
- **LLDAP:** Lightweight backend for users.
- **PocketID:** Beautiful OIDC Provider (Passkeys support).
- **TinyAuth:** Middleware that protects any web service with a login screen.
> **Magic:** The playbook automatically wires everything. It bootstraps LLDAP, logs into PocketID (via API), generates secrets, and configures TinyAuth without you touching the UI.

## 🏗 Architecture

- **OS Hardening:** Kernel tuning, UFW firewall, ZRAM, and custom routing hacks for anti-censorship.
- **Proxy:** Traefik v3 (Dynamic config, Wildcard SSL).
- **Autoheal:** Containers restart automatically if they feel sick.
- **Backups:** Restic + Rclone to S3 (via systemd timers, not cron).
- **Networking:** AmneziaWG (WireGuard UI) for VPN access.

## 📂 Structure
```
.
├── roles/
│   ├── bootstrap      \# First-time server setup
│   ├── common         \# Hardening, tools, routing
│   ├── docker         \# Engine \& Compose
│   ├── traefik        \# The Edge Router
│   ├── identity       \# LLDAP + PocketID + TinyAuth (Auto-wired)
│   ├── tuwunel        \# Conduit + LiveKit (Matrix Stack)
│   ├── amnezia        \# VPN
│   └── restic         \# Backup policies
├── site.yml           \# Main playbook
└── site-bootstrap.yml \# Initial run

```
## 🚀 Quick Start

### 1. Prepare
- Ansible on your machine.
- A fresh Debian/Ubuntu server.
- Copy `group_vars/all/vault.example.yml` to `vault.yml` and encrypt it with your secrets.

### 2. Bootstrap
Prepares users, SSH, and security settings.
```
ansible-playbook site-bootstrap.yml
```
### 3. Deploy Everything
Wires up the entire infrastructure.
```
ansible-playbook site.yml

```
### 4. Deploy specific parts
```
ansible-playbook site.yml --tags identity  \# Fix SSO
ansible-playbook site.yml --tags tuwunel   \# Update Matrix/LiveKit

```
## 🛡 Security Features
- **Automated Updates:** Watchtower keeps images fresh.
- **Strict Firewall:** UFW defaults to deny incoming.
- **Fail2Ban:** Standard SSH protection.
- **Healthchecks:** Every service has a Docker healthcheck wired to Traefik and Autoheal.
