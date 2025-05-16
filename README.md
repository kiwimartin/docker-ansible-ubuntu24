# Ansible Role: Docker Engine (Ubuntu 24.04 LTS)

Diese Ansible-Rolle installiert Docker Engine (inkl. Docker Compose & optional Rootless Mode) für Ubuntu 24.04 LTS.

## Verwendung

Erstelle ein Playbook, z.B. `site.yml`:

```yaml
- hosts: all
  become: true
  roles:
    - role: roles/docker
      vars:
        docker_install_mode: "rootless" # Oder "normal"
        docker_user: "martin"
```

## Variablen

Siehe `roles/docker/defaults/main.yml` für alle verfügbaren Variablen.

## Features

- Installation von Docker Engine, Compose, Buildx
- Optionale Installation und Aktivierung von Rootless Docker (systemd user units, linger)
- Handler für Neustart im Normalbetrieb

## Hinweise zu Rootless

- Nach Playbook-Run ist Rootless Docker für den gewählten Nutzer aktiviert.
- Systemd-Integration für rootless wird eingerichtet.
- Linger wird aktiviert, damit systemd user service auch nach Logout läuft.
