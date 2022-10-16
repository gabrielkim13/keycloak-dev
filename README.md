# Dev

Containerized development environment, using [Docker](https://www.docker.com/), with:

- [Active Directory (Samba)](https://www.samba.org/samba/)
- [Keycloak](https://www.keycloak.org/)
- [Oracle Database](https://www.oracle.com/database/)

## Getting started

To spin up the Docker containers for all services mentioned above, run:

```bash
docker-compose -p dev up -d
```

## Services

### Active Directory

> You can also use `DC=tecban,DC=com` as the Base DN, but beware that it will make overall usage of
> LDAP Admin and AD syncs **a lot** slower.

| Parameter   | Value                     |
| :---------- | :------------------------ |
| Port (LDAP) | 389                       |
| Base        | CN=Users,DC=tecban,DC=com |
| Username    | Administrator             |
| Password    | Pa$$w0rd                  |

#### Transport security

By default, Samba requires a secure connection in order to allow simple (i.e. authenticated) binds.
This means that for a system to be able to perform LDAP queries, LDAPS must be enabled.

To avoid the hassle of re-generating a self-signed certificate with the correct alt name entry and
adding its CA to every container / host that interacts with it, include the following lines to
`/var/lib/samba/private/smb.conf` and restart the container:

> References:
>
> - [[Samba] Allow unencrypted TLS LDAP query](https://lists.samba.org/archive/samba/2016-August/202204.html)
> - [ldap server require strong auth (G)](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html#LDAPSERVERREQUIRESTRONGAUTH)

```diff
# Global parameters
[global]
	dns forwarder = 127.0.0.11
	netbios name = TECBAN
	realm = TECBAN.COM
	server role = active directory domain controller
	workgroup = DEV-AD
	idmap_ldb:use rfc2307 = yes
+
+   client ldap sasl wrapping = sign
+   ldap server require strong auth = no

[sysvol]
	path = /var/lib/samba/sysvol
	read only = No

[netlogon]
	path = /var/lib/samba/sysvol/tecban.com/scripts
	read only = No

```

#### Recommended tools

- [LDAP Admin](https://sourceforge.net/projects/ldapadmin/)

### Keycloak

> In construction...

| Parameter | Value                  |
| :-------- | :--------------------- |
| URL       | http://localhost:8080/ |
| Username  | admin                  |
| Password  | admin                  |

#### LDAP integration

For more information regarding LDAP integration in Keycloak, read the following resources:

- [Server Administration Guide - LDAP](https://www.keycloak.org/docs/latest/server_admin/#_ldap)
- [Configuring Keycloak for active directory and LDAP integration](https://dmc.datical.com/administer/configure-keycloak-ldap.htm)

### Oracle Database

| Parameter    | Value                |
| :----------- | :------------------- |
| Port         | 1521                 |
| Service Name | ORCLPDB1             |
| Username     | SYS AS SYSDBA ou DEV |
| Password     | root                 |

#### Recommended tools

- [DBeaver](https://dbeaver.io/)
