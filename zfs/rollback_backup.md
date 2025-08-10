# Rolling Backups

Configuring & setting up rolling backups with ZFS.

We're focusing on a local pool but these steps are trivial to replicate for remote pools.

### Step 1: Install sanoid & syncoid

These are basically just scripts which should be on offer from your distro's package manager

### Step 2: Configuring sanoid

It by default lives in `/etc/sanoid/sanoid.conf`

A short sanoid configuration is probably:

```
[$primary_pool/$root_fs]
use_template = production
recursive = yes

[template_production]
frequently = 0
hourly = 0
daily = 90
monthly = 6
yearly = 1
autosnap = yes
autoprune = yes
```

Explaination:

* Keep 90 daily snapshots, e.g.: keep a snapshot around for 90 days.
* Keep 6 monthly snapshots, e.g.: keep monthly snapshots around for 6 months.
* Keep 1 yearly snapshot, e.g.: keep an image of the FS last year.


### Step 3: Create a timer

Systemd timers are a lot easier to work with than cron

```
[Unit]
Description=Triggers a daily snapshot
Requires=sanoid-snapshot.service zfs.target

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true
AccuracySec=1m
Unit=sanoid-snapshot.service

[Install]
WantedBy=timers.target
```

### Step 4: Create a sanoid unit file

Tricky thing here is `OnSuccess`, which triggers another unit file if this job succeeds. 

That was added in systemd v249, which is a little old at time of writing.

```
[Unit]
Description=Create ZFS snapshots with Sanoid
OnSuccess=syncoid-backup.service
Requires=zfs.target

[Service]
Type=oneshot
User=root
ExecStart=$path_to_sanoid --cron --verbose
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

### Step 5: Create syncoid unit file

```
[Unit]
Description=Sync ZFS snapshots with Syncoid
Requires=zfs.target

[Service]
Type=oneshot
User=root
ExecStart=$path_to_syncoid --quiet --debug --recursive --no-sync-snap $primary_pool/$root_fs $backup_pool/$root_fs
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

Point by point:

* `--quiet`: suppress progress/status bar output
* `--debug`: print debug information
* `--recursive`: `sanoid` config above is taking recursive snapshots, so we need to sync them recursively.
* `--no-sync-snap`: `sanoid` is managing our snapshots, so there is no reason for `syncoid` to do anything with them.
