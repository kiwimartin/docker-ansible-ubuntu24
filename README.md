# Ansible Role: Docker Engine (Ubuntu 24.04 LTS)

Diese Ansible-Rolle installiert Docker Engine (inkl. Docker Compose & optional Rootless Mode) für Ubuntu 24.04 LTS.

Erstellt mit Github Copilot.

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

Siehe `defaults/main.yaml` für alle verfügbaren Variablen.

### Docker-Daemon-Proxy

Ein Proxy für Image-Pulls und Registry-Zugriffe kann pro Host in `host_vars`
konfiguriert werden:

```yaml
docker_proxy:
  http_proxy: "http://proxy.example.org:3128"
  https_proxy: "http://proxy.example.org:3128"
  no_proxy: "localhost,127.0.0.1,.example.org"
```

Es werden nur Werte gesetzt, die nicht leer sind. Um die von der Rolle
verwaltete Proxy-Konfiguration wieder zu entfernen, alle Werte leer lassen
oder `docker_proxy` nicht setzen. Manuell angelegte systemd-Drop-ins werden
nicht verändert.

Die Proxy-Einstellungen gelten nur für den Docker-Daemon, nicht für APT oder
andere Installationsschritte der Rolle. Normaler und Rootless-Betrieb werden
unterstützt. Proxy-URLs mit Zugangsdaten sollten nicht im Klartext eingecheckt,
sondern zum Beispiel mit Ansible Vault verschlüsselt werden.

## Features

- Installation von Docker Engine, Compose, Buildx
- Optionale Installation und Aktivierung von Rootless Docker (systemd user units, linger)
- Handler für Neustart im Normalbetrieb

## Hinweise zu Rootless

- Nach Playbook-Run ist Rootless Docker für den gewählten Nutzer aktiviert.
- Systemd-Integration für rootless wird eingerichtet.
- Linger wird aktiviert, damit systemd user service auch nach Logout läuft.
