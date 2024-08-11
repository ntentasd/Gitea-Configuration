# My Gitea Configuration

<br><br>

## 1. Download the [gitea](https://dl.gitea.com/gitea/1.22.1/gitea-1.22.1-linux-amd64) binary

You can, also, do this via wget: `sudo wget https://dl.gitea.com/gitea/1.22.1/gitea-1.22.1-linux-amd64 -O /usr/local/bin/gitea`

Change the permissions: `sudo chmod 755 /usr/local/bin/gitea`

## 2. Create the gitea service

`nvim /etc/systemd/system/gitea.service`

Paste the following:

---
`[Unit]`

`Description=Gitea (Git with a cup of tea)`

`After=syslog.target`

`After=network.target`

`Wants=mysql.service`

`After=mysql.service`

---
`[Service]`

`RestartSec=2s`

`Type=simple`

`User=gitea`

`Group=gitea`

`WorkingDirectory=/var/lib/gitea`

`ExecStart=/usr/local/bin/gitea web --config /etc/gitea/app.ini`

`Restart=always`

`Environment=USER=gitea HOME=/home/gitea GITEA_WORK_DIR=/var/lib/gitea`

---
`[Install]`

`WantedBy=multi-user.target`

### Note that:

- You can pick the database of your choice; just replace `mysql` with `mariadb`, `postgesql`, `memcached` or `redis`
- You have to create the directories and the user:
	1. `sudo mkdir -p /var/lib/gitea`
	2. `adduser --system --disabled-password --group --shell /bin/bash --home /home/gitea gitea`
- You have to set ownerships and permissions:
	1. `chown -R gitea:gitea /var/lib/gitea`
	2. `chown -R gitea:gitea /run/gitea` #create 
	3. `chown -R root:gitea /etc/gitea` #create
	4. `chmod -R 750 /var/lib/gitea`
	5. `chmod 770 /etc/gitea`

---

# INCLUDE THESE NOTES TOO

- needs git
- CREATE DATABASE gitea;
- CREATE USER gitea WITH PASSWORD 'password';
- GRANT ALL PRIVILEGES ON DATABASE gitea TO gitea; (postgres)
- in app.ini psql must be "postgres"
- start and enable both services (gitea + nginx)
