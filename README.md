core-services
=============

Every tech organization's core services - GIT and Password manager.

Why?
----

To setup an infrastructure from code there are needed two independent services:
- GIT: To checkout the code
- Password manager: To decrypt secrets required to perform initial deployment

Architecture
------------

![architecture](./docs/architecture.png)

- Every service is a Podman container started by systemd
- **Gateway** is a reverse proxy that exposes services on standard 80 and 443 ports with a self-signed TLS certificate under valid domains, just in case of a Disaster Recovery. For a regular gateway use e.g. Kubernetes `kind Service` + `kind: Ingress` to expose an external service
- Static IP is assigned for each service
- Uses virtual network with constant subnet
- It is possible to place every service on a different machine, it would require a routing/VPN configuration and creation of a Podman subnet per machine
- Bind mount volumes are used to store application's data on host's disk at `/var/lib/riotkit-core`

Gitea
-----

Lightweight GIT server, easy to maintain, full of features.

**Purpose:** To be able to fetch cluster configuration in disaster recovery process

Passbolt
--------

Secure password manager with End-To-End (in browser) encryption using well know, audited GPG.

**Purpose:** To decrypt credentials stored in GIT, required during deployment time

**Notice:** The Passbolt image is built on runtime, old images are deleted. Reason for this is to add a PostgreSQL library to be able to connect to database.

Gateway
-------

Simple HTTP and HTTPS reverse proxy. Exposes services under given domain/subdomain names. Uses self-signed certificates for TLS.

**Purpose:** Access Gitea and Passbolt simply by switching IP address in `/etc/hosts` during recovery process

PostgreSQL
----------

A dependency to Passbolt and Gitea.

Enjoy!
------

We are a grassroot, tech collective and made this project to fulfill needs of anarchist, anticapitalist, grassroot collectives.
