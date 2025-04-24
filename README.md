# Keycloak + HA PostgreSQL on Docker Swarm
A **one-file stack** that launches the latest Keycloak (26.2.1) backed by a **three-node PostgreSQL 17 cluster** (Bitnami `postgresql-repmgr`).  
The Keycloak container is totally stateless, so the scheduler can move it to any node without fuss.

---

## Why you might want this

* **Batteries included:** automatic leader election, streaming replication & fail-over via repmgr.  
* **Swarm-native:** overlay network, named volumes, secrets – nothing fancy required.  
* **Zero mounts on Keycloak:** scale it horizontally or reschedule it anywhere.  
* **Old-school transparent:** no Helm, no Operator – just plain docker-compose like Torvalds would read.

---

## Quick start

```bash
# 1) Fire up a Swarm (skip if you already have one)
docker swarm init

# 2) Create the required secrets (edit the values to taste)
echo -n 'SuperSecret1' | docker secret create KC_DB_PASSWORD -
echo -n 'kcadmin'      | docker secret create KEYCLOAK_ADMIN -
echo -n 'AdminSecret2' | docker secret create KEYCLOAK_ADMIN_PASSWORD -
echo -n 'RepmgrPass3'  | docker secret create REPMGR_PASSWORD -
echo -n 'RootDbP4ss'   | docker secret create PG_SUPERUSER_PASSWORD -

# 3) Deploy the stack
docker stack deploy -c docker-compose.yml keycloak

Point your browser to **`http://<any-swarm-node>:8080`** (or whatever port you expose) and log in with the `KEYCLOAK_ADMIN` creds you set above.

---

## Scaling

```bash
# Want three Keycloak pods?
docker service scale keycloak_keycloak=3
```

Stick Traefik, HAProxy, or your LB of choice in front if you need sticky sessions.

---

## File structure

```
docker-compose.yml   # the only file you need
README.md            # this doc
```

---

## Contributing

Pull requests, issues, and brutally honest code reviews are **very welcome**.  
Got a nicer way to wire the secrets, or fancy adding CI? Have at it!

---

## License

This project is released under the **GNU General Public License, version 2** (GPL-2.0).

> “Free as in freedom, not as in free beer.”

Happy hacking!
