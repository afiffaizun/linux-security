# Linux Security - UFW (Uncomplicated Firewall)

UFW is an easy-to-use firewall management tool on Linux, built on top of iptables.

---

## Command List (Correct Order)

| No | Command | Description |
|----|---------|-------------|
| 1 | `sudo ufw status` | Check current firewall status |
| 2 | `vim /etc/default/ufw` | Edit main configuration file |
| 3 | `sudo ufw reset` | Reset all rules |
| 4 | `sudo ufw default deny incoming` | Deny all incoming connections |
| 5 | `sudo ufw default allow outgoing` | Allow all outgoing connections |
| 6 | `sudo ufw allow ssh` | Open SSH port (22) |
| 7 | `sudo ufw allow http` | Open HTTP port (80) |
| 8 | `sudo ufw allow https` | Open HTTPS port (443) |
| 9 | `sudo ufw allow ftp` | Open FTP port (21) |
| 10 | `sudo ufw allow from 10.x.x.x` | Allow a specific IP address |
| 11 | `sudo ufw allow from 100.0.1 to any port 22` | Allow specific IP to port 22 |
| 12 | `sudo ufw deny ftp from <IP>` | Block FTP from a specific IP |
| 13 | `sudo ufw enable` | Enable UFW |
| 14 | `sudo systemctl restart ufw` | Restart UFW service |
| 15 | `sudo ufw status verbose` | Check detailed status |
| 16 | `sudo ufw status numbered` | Show numbered rules |
| 17 | `sudo ufw delete rule 5` | Delete rule number 5 |

---

## Detailed Explanation

### 1. `sudo ufw status`
Check the current state of the firewall before making any changes.
- Shows whether UFW is active or inactive
- Displays existing rules
- Output: `Status: active` or `Status: inactive`

---

### 2. `vim /etc/default/ufw`
Edit the main UFW configuration file.
- Configure basic UFW settings (IPv6, default policy, etc.)
- Use `:q` to exit without saving
- Use `:wq` to save and exit

---

### 3. `sudo ufw reset`
Reset all rules back to default (empty state).
- Deletes all previously created rules
- UFW will be disabled after reset
- A confirmation prompt y/n will appear

> Warning: All previous configurations will be lost.

---

### 4. `sudo ufw default deny incoming`
Deny all incoming connections by default.
- All unauthorized traffic from outside will be blocked
- Highly recommended security practice
- All ports are locked first, then opened one by one as needed

---

### 5. `sudo ufw default allow outgoing`
Allow all outgoing connections by default.
- The machine can freely access the internet or external networks
- Browsing, downloads, and updates will still work normally

---

### 6. `sudo ufw allow ssh`
Open port 22 for SSH connections.
- SSH is used for remote login to the server
- Default SSH port = 22

> Important: This must be done before enabling UFW to prevent losing remote access.

---

### 7. `sudo ufw allow http`
Open port 80 for HTTP traffic.
- Used for standard websites (unencrypted)
- Required if the server runs a web server such as Apache or Nginx
- Default HTTP port = 80

---

### 8. `sudo ufw allow https`
Open port 443 for HTTPS traffic.
- Used for websites with SSL/TLS encryption
- More secure than plain HTTP
- Default HTTPS port = 443

---

### 9. `sudo ufw allow ftp`
Open port 21 for FTP traffic.
- FTP is used for file transfers between computers
- Default FTP port = 21

> Note: FTP is unencrypted. Consider using SFTP for better security.

---

### 10. `sudo ufw allow from 10.x.x.x`
Allow connections from a specific IP address.
- Only the specified IP will be allowed in
- Commonly used for local or internal networks

---

### 11. `sudo ufw allow from 100.0.1 to any port 22`
Allow a specific IP to access port 22 (SSH).
- Only IP `100.0.1` is permitted to make SSH connections
- `to any` means to all network interfaces
- More secure by restricting who can SSH into the server

---

### 12. `sudo ufw deny from <IP> to any port 21`
Block FTP from a specific IP address.
- Rejects FTP connections from the specified IP
- Useful for blocking suspicious IP addresses
- A deny rule overrides an allow rule when more specific

---

### 13. `sudo ufw enable`
Enable UFW.
- Activates the firewall with all configured rules
- UFW will automatically start on system reboot

> Make sure SSH is allowed before running this command.

---

### 14. `sudo systemctl restart ufw`
Restart the UFW service.
- Reloads all UFW configurations
- Should be done after all rules have been added
- Ensures all changes are properly applied

---

### 15. `sudo ufw status verbose`
Check UFW status in detail.
- Displays all active rules
- Shows default policy (allow/deny)
- More detailed than plain `ufw status`

---

### 16. `sudo ufw status numbered`
Display rules with sequence numbers.
- Each rule is assigned a number (1, 2, 3, ...)
- Makes it easier to delete a specific rule

Example output:
```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     ALLOW IN    Anywhere
[ 3] 443/tcp                    ALLOW IN    Anywhere
```

---

### 17. `sudo ufw delete rule 5`
Delete rule number 5.
- Removes a rule based on its sequence number
- The number is obtained from the `status numbered` command
- After deletion, verify again with `status numbered`

---

## Workflow Summary

```
Check Status
    |
Edit Configuration
    |
Reset (optional)
    |
Set Default Policy (deny incoming, allow outgoing)
    |
Open Required Ports (SSH first!)
    |
Enable UFW
    |
Restart UFW
    |
Verify Status
    |
Edit Rules if Needed (delete/add)
```

---

## Tips & Best Practices

| Tip | Description |
|-----|-------------|
| Allow SSH before enabling | Prevents being locked out of the server |
| Default deny incoming | Better security posture |
| Check status after changes | Verify rules are applied correctly |
| Be careful when resetting | All rules will be deleted |
| Use SFTP instead of FTP | FTP is unencrypted |

---

## Common Port Reference

| Service | Port | Protocol |
|---------|------|----------|
| SSH | 22 | TCP |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| FTP | 21 | TCP |
| MySQL | 3306 | TCP |
| PostgreSQL | 5432 | TCP |

---

*Created based on Linux Security - UFW notes*
