# Practice 9 — Grafana Authentication via FreeIPA (LDAP)

## Context


## 1. FreeIPA server

```bash
dnf install ipa-server ipa-server-dns -y
dnf install openldap-clients -y

# Enable firewall
firewall-cmd --permanent --add-service={freeipa-4,http,https,dns}
firewall-cmd --reload

ipa-server-install
```

Verify and create the LDAP user Grafana will bind as / authenticate:

```bash
kinit admin
klist
ipa user-show admin
ipa user-add daniel --first=daniel --last=delax --email=daniel@example.com
ipa passwd daniel
ipa config-show
```

## 2. Name resolution (`/etc/hosts`)

```
10.x.x.x   daniel.delax.com   daniel
```

## 3. Grafana in Docker, with LDAP auth

Install Docker:

```bash
dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
systemctl enable docker
systemctl start docker
```

Prepare the data/config directories:

```bash
mkdir -p /opt/grafana/{data,config}
chown -R 472:472 /opt/grafana/data   # 472 is the "grafana" UID/GID in the official image
```

Write [`docker-compose.yml`](docker-compose.yml) and [`ldap.toml`](ldap.toml)
(both included in this folder), then lock down the LDAP config file:

```bash
vi docker-compose.yml
vi ldap.toml
chmod 644 /opt/grafana/ldap.toml
chown root /opt/grafana/ldap.toml

docker compose up -d
```

Useful commands while iterating:

```bash
docker compose ps
docker compose down
docker logs grafana-ldap --tail 40 | grep -iE '(ldap|bind|error|tls)'
```

### LDAP config highlights ([`ldap.toml`](ldap.toml))

- Binds to the FreeIPA server over `start_tls` on port 389.
- `search_filter = "(uid=%s)"` against
  `cn=users,cn=accounts,dc=delax,dc=com`.
- Group-to-role mapping: members of `grafana-admins` become Grafana
  **Admin**, `grafana-editors` become **Editor**, everyone else falls
  through to **Viewer**.

## Summary

- Deployed FreeIPA as the identity provider and created a test user.
- Ran Grafana in Docker with a bind-mounted `ldap.toml`, configured for `start_tls` and group-based role mapping.
- Verified the LDAP integration through Grafana's built-in connection test and `docker logs`.
