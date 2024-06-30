# via Docker
docker pull quay.io/keycloak/keycloak:latest

# Local
docker run -d --name keycloak -p 8020:8020 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin keycloak/keycloak:latest start-dev

# Production without SSL
start --auto-build --db=dev-mem --hostname localhost:8080 --http-enabled true --hostname-strict-https false

# Production
start --auto-build --hostname=hexadefence.com --db=postgres --features=token-exchange --db-url=<JDBC-URL> --db-username=<DB-USER> --db-password=<DB-PASSWORD> --https-key-store-file=<file> --https-key-store-password=<password>

```
Open http://localhost:8080/auth/
Click on the “Administration Console” link.
Log in using admin for both username and password.

docker build --build-arg KEYCLOAK_VERSION=21.1.1 -t keycloak --progress=plain --no-cache .
docker build -t keycloak --progress=plain --no-cache .
docker rm -f mykeycloak
docker logs -f mykeycloak
docker run -d --name mykeycloak -p 8080:8080 \
         -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=password \
         keycloak \
         --verbose \
         start \
         --hostname-strict=false \
         --optimized \
         --http-enabled=true \
         --db-url="jdbc:postgresql://postgres/keycloak" \
         --db-username=keycloak \
         --db-password=password

docker compose build --no-cache keycloak
docker compose up -d
```