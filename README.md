Purpose:
Define minimum user and group privileges for a small development team to limit blast radius and enable auditing.

Roles:
- admin: System administrators who need full root access for maintenance. Membership by exception, approved by manager.
- dev: Developers who need access to development files and ability to restart app services. Not allowed full sudo; specific allowed commands granted.
- auditor: Read-only access to system and logs; no sudo.
- ops: Operators allowed to manage deployments (service restart, view app logs) — limited sudo for specific commands.

Who gets sudo:
- admin group: full sudo (with password) via /etc/sudoers.d/admin (require strong password and MFA where possible).
- dev group: allowed to run only specific commands as root (e.g., /bin/systemctl restart app.service, /usr/bin/journalctl -u app.service) — set via separate sudoers entries.
- ops group: subset of dev commands (restart, tail logs).
- No NOPASSWD for interactive shells. NOPASSWD allowed only for clearly justified non-interactive service scripts.

Account & password policy:
- Disable root SSH login.
- Use strong passwords; prefer SSH keys for login.
- Expire dormant accounts after X days (e.g., 90 days) and require approval to re-enable.

Shared project folder:
- Group: project_team (contains dev + ops).
- POSIX perms + ACLs: project_team has rwX, others have rX.

Auditing:
- Enable auditd to watch /etc/passwd, /etc/sudoers, /etc/sudoers.d and critical cron dirs. Alerts for file modifications.

Onboarding/offboarding checklist:
- Create user, add to groups, generate SSH key, set password expiry, document access.
- Offboarding: remove from groups, disable account (usermod -L), set shell to /sbin/nologin, archive home, rotate passwords if shared.

Review cadence:
- Sudoers & group membership reviewed quarterly.
