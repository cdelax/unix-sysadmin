# Linux Practices

A collection of hands-on Linux administration exercises: user & permission
management, storage (LVM/NFS), automation (Ansible), networking (bonding/
VLANs), name services (DNS, FreeIPA/LDAP), and running services in Docker
with LDAP-backed authentication (Grafana).

Each exercise lives in its own folder with the original prompt, the
commands/config that were used, and evidence screenshots where available.

## Index

| #                                 | Practice                 | Topics                                                        | Status                |
| --------------------------------- | ------------------------ | ------------------------------------------------------------- | --------------------- |
| [01](01-users-and-acls/)          | Users & ACLs             | `useradd`, `usermod`, groups, `setfacl`/`getfacl`, ACL masks  | ✅                     |
| [02](02-nfs-hard-and-soft-links/) | NFS & Hard/Soft Links    | NFS server/export, `/etc/fstab`, hard vs. soft links, inodes  | ✅                     |
| [03](03-lvm/)                     | LVM                      | `vgcreate`, `lvcreate`, `mkfs`, `lvreduce`/`lvextend`, swap   | ✅                     |
| [04](04-ansible-lvm-automation/)  | Ansible Automation       | Playbooks that build and tear down Practice 3                 | ✅                     |
| [05](05-networking-bonding-vlan/) | Networking               | Netplan, virtual interfaces, NIC bonding, tagged VLANs        | ✅                     |
| [06](06-local-repositories/)      | Local Repositories       | Local `dnf` repo served over HTTP, no subscription-manager    | ⚠️ notes not captured |
| [07](07-dns-server/)              | DNS Server               | BIND, zone records, `nslookup`/`dig`, reverse lookups         | ✅                     |
| [08](08-autofs/)                  | Autofs                   | On-demand NFS home directory mounts                           | ⚠️ notes not captured |
| [09](09-grafana-freeipa-ldap/)    | Grafana + FreeIPA (LDAP) | FreeIPA server, Docker, Grafana LDAP auth, group→role mapping | ✅                     |

Practices marked ⚠️ only have the original exercise prompt preserved — the
commands that were actually run weren't recorded in the source notes. A
reference outline is included as a starting point; replace it with what was
actually done.

## Lab environment

Exercises were run across a small set of RHEL-family lab servers (referred
to as `sacvl5199`/`sacvl5200`, `k-lvl5199`, `gdlvl5200`, etc.), generally
in pairs so that server-side and client-side steps could be practiced
together (e.g. NFS export on one host, mount on the other).

## Before publishing this repo publicly

A couple of exercises carry over lab-only credentials and internal
hostnames from the original notes (see the ⚠️ note in
[Practice 9](09-grafana-freeipa-ldap/README.md)). If this repo is going
somewhere public (e.g. as a portfolio piece), rotate/redact those first.
