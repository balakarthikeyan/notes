# GitLab Local Setup & Troubleshooting Guide (Docker + Windows 11) & GitLab Runner Configuration Documentation

Create a docker-compose.yml file with the provided configuration.

Ensure proper file permissions for config, logs, data, postgres, and ssl folders.

This documentation explains the purpose of the provided commands and their usage in setting up GitLab and configuring GitLab Runner.

1. Restarting GitLab Container

`docker restart gitlab-local`

2. Checking GitLab Logs

`docker logs -f gitlab-local`

3. Checking if GitLab is Listening on Port 10443

`docker exec -it gitlab-local netstat -tulnp | grep 10443`
or
`docker exec -it gitlab-local netstat -ano | findstr :10443`

Checks if GitLab’s Nginx server is listening on port 10443 (HTTPS) inside the container. If no output appears, GitLab is not binding to port 10443.

4. Verifying GitLab Nginx Configuration

`docker exec -it gitlab-local cat /etc/gitlab/gitlab.rb | findstr nginx`

Displays all nginx-related settings in gitlab.rb.

5. Editing GitLab Configuration

`docker exec -it gitlab-local vi /etc/gitlab/gitlab.rb`

You can manually check or modify configurations inside the container.

6. Stopping and Restarting Docker Containers

`docker-compose down`

Stops and removes all running GitLab containers.

`Restart-Computer -Force`

Reboots the system (useful if Docker has networking issues).

`docker-compose up -d --build`
or
`docker compose up -d --build --force-recreate`

Rebuilds and restarts the GitLab container with fresh configurations.

8. Retrieving GitLab's Root Password

`docker exec -it gitlab-local cat /etc/gitlab/initial_root_password`

Displays the default root password created on first startup.

9. Checking GitLab Directories and SSL Files

- `docker exec -it gitlab-local ls -l /etc/gitlab`
- `docker exec -it gitlab-local ls -l /var/opt/gitlab`
- `docker exec -it gitlab-local ls -l /etc/gitlab/ssl/`

Lists contents of key GitLab directories:
- `/etc/gitlab/` → Main configuration directory.
- `/var/opt/gitlab/` → Stores GitLab data.
- `/etc/gitlab/ssl/` → Contains SSL certificates.

10. Copying SSL Certificates into the GitLab Container

`docker cp ./ssl/gitlab.local.crt gitlab-local:/etc/gitlab/ssl/`

`docker cp ./ssl/gitlab.local.key gitlab-local:/etc/gitlab/ssl/`

Copies your SSL certificate (gitlab.local.crt) and private key (gitlab.local.key) into the GitLab container.

`docker exec -it gitlab-local chmod 700 /etc/gitlab/ssl/gitlab.*`

Sets correct permissions for SSL files.

11. Applying GitLab Configuration Changes

`docker exec -it gitlab-local gitlab-ctl reconfigure`

Reconfigures GitLab based on gitlab.rb changes.

`docker exec -it gitlab-local gitlab-ctl restart`

Restarts GitLab services.

12. Testing GitLab HTTPS Access

`docker exec -it gitlab-local curl -v -k https://localhost:10443`

Checks if GitLab responds to HTTPS requests inside the container.

`-k` → Ignores SSL certificate warnings.

13. Checking SSL Directory in GitLab

`docker exec -it gitlab-local ls /etc/gitlab/ssl/`

14. Fixing DNS Issues

`ipconfig /flushdns` and
`Restart-Service Dnscache`

15. Opening Firewall for GitLab

`New-NetFirewallRule -DisplayName "Allow GitLab 10443" -Direction Inbound -Protocol TCP -LocalPort 10443 -Action Allow`

Creates a Windows Firewall rule to allow inbound HTTPS traffic (port 10443).

`Get-NetFirewallRule | Where-Object DisplayName -eq "Allow GitLab 10443"`

Verifies if the firewall rule was successfully created.

16. Disabling & Enabling Windows Firewall (For Debugging)

`netsh advfirewall set allprofiles state off`

Temporarily disables Windows Firewall (useful for testing network issues).

`netsh advfirewall set allprofiles state on`

Re-enables Windows Firewall after testing.

# Fix for GitLab Runner Configuration
Mount a econfigured config.toml

1. Manually Register GitLab Runner

`docker exec -it gitlab-runner gitlab-runner register`

2. Provide the Following Details When Prompted:

- `GitLab Runner Token:` Get this from GitLab UI → Admin Area → Runners.
- `Description:` local-runner
- `Tags:` (Optional, e.g., docker, local, test)
- `Executor:`
  - Choose "docker" if you want jobs to run in Docker.
  - Choose "shell" if you want them to run directly on the host machine.

3. Register GitLab Runner Again

`docker exec -it gitlab-runner gitlab-runner register --url https://gitlab.local:10443 --token YOUR_RUNNER_TOKEN`

de-register
`docker exec -it gitlab-runner gitlab-runner unregister --all-runners`

4. Fix the Warning in config.toml

`docker exec -it gitlab-runner vi /etc/gitlab-runner/config.toml`

or

`docker cp ./gitlab-runner/config.toml gitlab-runner:/etc/gitlab-runner/config.toml`

Then restart the GitLab Runner:

`docker restart gitlab-runner`

5. Verify the Runner Again

`docker exec -it gitlab-runner gitlab-runner list`

# Fix GitLab Runner TLS Verification Issue
Since GitLab is using a self-signed certificate, GitLab Runner does not trust it by default. We need to manually add the certificate.

1. Copy the GitLab SSL Certificate into GitLab Runner

`docker cp gitlab-local:/etc/gitlab/ssl/gitlab.local.crt gitlab-runner:/etc/gitlab-runner/certs/`

or

If you haven't already done so, copy the SSL certificate from gitlab-local to your host:

`docker cp gitlab-local:/etc/gitlab/ssl/gitlab.local.crt .`

Now, copy it to gitlab-runner:

`docker cp gitlab.local.crt gitlab-runner:/etc/gitlab-runner/certs/`

Now, copy it to ca-certificates:

`docker cp gitlab.local.crt gitlab-runner:/usr/local/share/ca-certificates/gitlab.local.crt`

2. Update Certificate Trust in GitLab Runner

Run the following inside the gitlab-runner container:
`docker exec -it gitlab-runner update-ca-certificates`

Restart the runner:
`docker restart gitlab-runner`

Ping and check
`docker exec -it gitlab-runner ping -c 4 gitlab.local`

3. Re-register GitLab Runner

```bash
docker exec -it gitlab-runner gitlab-runner register --non-interactive \
  --url "https://gitlab.local:10443" \
  --registration-token "glrt-t3_9W5DWw9joe_My-zwX4CW" \
  --executor "docker" \
  --description "Local GitLab Runner" \
  --docker-image "alpine:latest" \
  --tls-ca-file "/etc/gitlab-runner/certs/gitlab.local.crt"
```

```bash
docker exec -it gitlab-runner gitlab-runner register --non-interactive `
  --url "https://gitlab.local:10443" `
  --registration-token "glrt-t3_9W5DWw9joe_My-zwX4CW" `
  --executor "shell" `
  --description "Shell Runner" `
  --tag-list "shell" `
  --run-untagged="true" `
  --locked="false"
```

4. Verify GitLab Runner

After registering, check if it appears in the list:

`docker exec -it gitlab-runner gitlab-runner list`

If successful, you should see something like:

```bash
Listing configured runners
GitLab Runner  Executor=docker  Token=XXXXXX  URL=https://gitlab.local:10443
```

5. Restart GitLab Runner

To make sure everything is applied, restart the runner again:
`docker restart gitlab-runner`

6. Check for Jobs

`docker exec -it gitlab-runner gitlab-runner run`

If everything is set up correctly, your runner should now be able to fetch jobs and execute pipelines.

7. Set Folder Permissions (Windows)

`icacls config logs data ssl /grant Everyone:F /T /C /Q `

*Purpose:* Grants full control (F) permissions to everyone for the folders config, logs, data, and ssl.

Flags:
- `/T:` Applies changes to all subdirectories and files.
- `/C:` Continues execution even if errors occur.
- `/Q:` Suppresses success messages.

*Why?:* Ensures GitLab has the necessary permissions to read/write these folders.

8. Generate SSH Key for GitLab

`ssh-keygen -t rsa -b 4096 -C "balakarthikeyan07@gmail.com"`

*Purpose:* Generates an SSH key (RSA 4096-bit) for secure authentication.

Flags:
- `-t` rsa: Specifies the key type as RSA.
- `-b` 4096: Uses 4096-bit encryption (stronger security).
- `-C` "balakarthikeyan07@gmail.com": Adds a comment (your email) for identification.

9. Generate Self-Signed SSL Certificate

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout gitlab.local.key -out gitlab.local.crt -subj "/C=US/ST=State/L=City/O=Org/CN=gitlab.local" -addext "subjectAltName=DNS:gitlab.local,IP:127.0.0.1"`

*Purpose:* Creates a self-signed SSL certificate for gitlab.local.

Flags:
- `x509:` Creates an X.509 certificate (SSL).
- `nodes:` No password protection on the private key.
- `days 365:` Valid for 1 year.
- `newkey rsa:2048:` Generates a 2048-bit RSA key.
- `keyout gitlab.local.key:` Saves the private key.
- `out gitlab.local.crt:` Saves the certificate.
- `subj "/C=US/ST=State/L=City/O=Org/CN=gitlab.local":` Sets certificate subject details.
- `addext "subjectAltName=DNS:gitlab.local,IP:127.0.0.1":` Adds Subject Alternative Names (SAN) for SSL validation.

10. Register GitLab Runner

`docker exec -it gitlab-runner gitlab-runner register`

*Purpose:* Starts the GitLab Runner registration process inside the gitlab-runner container.

*Why?:* Necessary to connect the runner with GitLab and allow it to execute CI/CD jobs.

11. Open Bash Shell in GitLab Runner Container

`docker compose exec -it gitlab-runner /bin/bash`

*Purpose:* Opens an interactive Bash shell inside the gitlab-runner container.

*Why?:* Useful for troubleshooting, debugging, and manually modifying configurations.

12. Manually Register GitLab Runner

`gitlab-runner register --url https://gitlab.local:10443 --token glrt-t3_9W5DWw9joe_My-zwX4CW`

*Purpose:* Registers the GitLab Runner with GitLab.

Flags:
- `--url https://gitlab.local:10443:` GitLab instance URL.
- `--token glrt-t3_9W5DWw9joe_My-zwX4CW:` Registration token provided by GitLab.

13. Start the GitLab Runner

`gitlab-runner run`

*Purpose:* Manually starts the GitLab Runner to fetch and execute CI/CD jobs.

14. Create network

`docker network create gitlab_network`

15. Clone local project:

`git clone ssh://git@gitlab.local:2224/root/myproject.git .`

Test the runner  
`docker exec -it gitlab-runner git clone https://gitlab.local:10443/root/myproject.git`