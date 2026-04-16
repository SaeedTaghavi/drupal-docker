# Drupal Site
This repository contains a Dockerized sandbox environment for exploring and learning Drupal 10.

## Prerequisites
- Docker and Docker Compose installed.
- Ports 80 and 3306 available on the host machine.

## Setup & Installation
Follow these steps to get your local development environment running:

## 1. Environment Configuration
Clone the repository and create your local environment file:
```Bash
cp .env.example .env.local
```
Update the variables in `.env.local` with your preferred database credentials and project name.

## 2. Prepare Local Directories
Create the folders mapped as bind mounts for custom development:

```Bash
mkdir -p web/themes web/modules web/profiles
```
## 3. Launch Containers
Start the stack using the specific environment file:

```Bash
docker compose --env-file .env.local up -d
```
## 4. Drupal Installation Wizard
- Open your browser to http://localhost:port.

- Follow the setup wizard.

- Database Configuration (Advanced Options):
    1. Host: database (should match the name of the database service in docker-compose.yml)
    2. Database Name: (from your .env.local)
    3. Username/Password: (from your .env.local)

## Management Commands
### Stopping the Site
To stop the containers without losing data:

```Bash
docker compose --env-file .env.local stop
```
### Restarting / Applying Changes
If you modify the docker-compose.yml or .env.local:

```Bash
docker compose --env-file .env.local up -d
```
### Resetting the Environment (Full Wipe)
Warning: This deletes the database and all uploaded files permanently.

```Bash
docker compose --env-file .env.local down -v
```
**Note on Permissions:** If you manually delete the web directory, you may need to use `sudo` because Docker maps container users (like www-data) to the host filesystem.

## Project Structure
- /web/themes: Place custom lab themes here.
- /web/modules: Place custom or contributed modules here.
- .env.local: Local secrets (ignored by git).
- drupal_sites: (Named Volume) Managed by Docker; contains settings.php and public files.

## Security Notes
Never commit `.env.local` or any file containing real passwords to version control.

Ensure `web/sites/default/settings.php` permissions are set to read-only (644) after the installation is complete.




## Learning To-Do List
- [x] Initial Docker Setup (Apache)
- [ ] Migrate to NGINX + PHP-FPM
  - [ ] Update `docker-compose.yml` with `drupal:10-fpm`
  - [ ] Add `nginx` service
  - [ ] Write a custom `nginx.conf`
- [ ] Set up Docker Secrets for DB passwords
- [ ] Create a database backup script
- [ ] Create a dedicated database backup service
    - [ ] Write `scripts/backup.sh` with rotation logic (delete >7 days)
    - [ ] Create `scripts/crontab` for automated scheduling
    - [ ] Build `Dockerfile.backup` with `cron` and `mariadb-client`
    - [ ] Add `backup-worker` sidecar service to `docker-compose.yml`
    - [ ] Test manual execution via `docker compose exec`
- [ ] Custom Development
  - [ ] Create a "Hello World" custom module
  - [ ] Build a custom sub-theme
