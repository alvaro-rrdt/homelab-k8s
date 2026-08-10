# Backup & Restore Runbook

## What's backed up
- /root/vault (Obsidian vault)
- /var/lib/rancher/k3s/storage/ (PVCs: ActualBudget + Forgejo data)

## Where
- Hetzner Storage Box (BX11, 1TB), SFTP port 23, user u648454

## How
- Tool: restic (encrypted, deduplicated)
- Script: /usr/local/bin/backup.py (reads password from /etc/restic-pw)
- Schedule: nightly 02:30 UTC, systemd timer restic-backup.timer

## Restore
- restic -r "sftp:u648454@u648454.your-storagebox.de:23/backups" snapshots
- restic -r "sftp:u648454@u648454.your-storagebox.de:23/backups" restore latest --target /tmp/restore-test

## Lessons learned
- Single-colon sftp:host:23/path is what works with this restic version + Storage Box (verified on disk)
- Storage Box password ≠ Hetzner login (reset in console if lost)
- ALWAYS test restores — "IDENTICAL" is the only proof that matters
