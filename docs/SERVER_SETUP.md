# Server Operations & Security Setup

This document outlines the server architecture, permission model, and deployment procedures used in the production enviroment (`/opt/homeserver`).

## 1. Directory Structure
* **Project Location: ** `/opt/homeserver/`
* **Reasoning:** Moved from `/home/user` to `/opt` to enable safe multi-user collaboration without exposing user home directories.
## 2. Permission Model (Collaboration)
To allow multiple users (e.g., `webadmins` group) to edit files without permission conflicts, we utilize **SGID (Set Group ID)**.

| Role | User/Group | Permissions | Scope |
| :--- | :--- | :--- | :--- |
| **Owner** | `root` | `rwx` (7) | Full Control |
| **Group** | `webadmins` | `rwx` (7) | Edit Code & Configs |
| **Others** | `*` | `r-x` (5) | Read-only (Required for Nginx/Docker) |

### Setup Commands Used
```bash
# 2.1  Ownership
sudo chown -R root:webadmins /opt/homeserver

# 2.2 Permissions (775 to allow Nginx read access)
sudo chmod -R 775 /opt/homeserver

# 2.3 SGID (Magic Bit)
# Ensures new files inherit 'webadmins' group automatically
sudo chmod g+s /opt/homeserver

##3.  Deployment & Updates
To apply changes to the live server:

```bash
cd /opt/homeserver
docker compose down
docker compose up -d

## 4. Troubleshooting

**Error 403 Forbidden (Nginx)**
Usually caused by strict permissions preventing the container from reading files.
* **Fix:** `sudo chmod -R 775 /opt/homeserver`

**Docker Network Stuck**
If `docker compose down` hangs or network removal fails:
* **Fix:** `sudo systemctl restart docker`

