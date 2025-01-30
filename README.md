# IGRP Platform Docker Compose

Repository to manage and populated a testing environment with all the required connectivity between User Management API, App Manager API, Keycloak and their DB. 

Notice that all the connections and variables are setted in the `.env` file, thereby modifying will have an impact in the whole system.

# Hosts file
Before starting the containers, you must specify the hostnames in your local machine for the local DNS to resolve them, otherwise you won't be able to use them. This can be do so by modifying the hosts file in your machine, which varies depending on the OS. 

```
127.0.0.1 keycloak
127.0.0.1 igrp-ui
127.0.0.1 user-management-api
127.0.0.1 app-manager-api
127.0.0.1 minio
```

# Environment Variables Documentation

## Database Configuration
- **POSTGRES_DB**: The name of the PostgreSQL database.  
  Example: `igrp-db`
- **POSTGRES_USER**: The username for connecting to the PostgreSQL database.  
  Example: `igrp`
- **POSTGRES_PORT**: The port on which PostgreSQL is running.  
  Example: `5432`
- **POSTGRES_PASSWORD**: The password for the PostgreSQL user.  
  Example: `1234`
- **POSTGRES_HOSTNAME**: The hostname or service name of the PostgreSQL server in Docker Compose.  
  Example: `db`

---

## User Management Service
- **USER_MANAGEMENT_PORT**: The port where the User Management API is exposed.  
  Example: `8081`
- **USER_MANAGEMENT_URL**: The URL for accessing the User Management API, constructed using the hostname and port.  
  Example: `http://user-management-api:8081`

---

## Keycloak Configuration
- **KEYCLOAK_ADMIN**: Keycloak administrator username.  
  Example: `admin`
- **KEYCLOAK_ADMIN_PASSWORD**: Keycloak administrator password.  
  Example: `password`
- **KEYCLOAK_HOSTNAME**: The service name or hostname of the Keycloak server.  
  Example: `keycloak`
- **KEYCLOAK_HOSTNAME_PORT**: The port where Keycloak is running.  
  Example: `8080`
- **KEYCLOAK_REALM**: The realm name configured in Keycloak.  
  Example: `quickstart`
- **KEYCLOAK_CLIENT_ID**: The client ID used for frontend authentication with Keycloak.  
  Example: `whole-api`
- **KEYCLOAK_CLIENT_SECRET**: The client secret for authenticating with the Keycloak client.  
  Example: `wpq7oMFUPiAFZIXky3TRzl7kU6zmsHs0`

---

## MinIO Configuration
- **MINIO_URL**: The URL for accessing the MinIO server.  
  Example: `http://minio:9000`
- **MINIO_PORT**: The port on which MinIO is running.  
  Example: `9000`
- **MINIO_SECURITY**: Flag indicating whether MinIO runs in secure mode.  
  Example: `false`
- **MINIO_ACCESS_KEY**: The access key for MinIO authentication.  
  Example: `admin`
- **MINIO_SECRET_KEY**: The secret key for MinIO authentication.  
  Example: `admin12345678`
- **MINIO_BUCKET_NAME**: The name of the bucket to be used in MinIO.  
  Example: `igrp`

---

## App Manager Configuration
- **APP_MANAGER_URL**: The URL for accessing the App Manager API.  
  Example: `http://app-manager-api:8082`
- **APP_AUTHENTICATION_PROVIDER**: The authentication provider for the application.  
  Example: `keycloak`

---

## Email Server Configuration
- **EMAIL_SENDER**: The email address used as the sender in email communications.  
  Example: `javier.merida@wayvant.io`
- **EMAIL_HOST**: The email server's hostname.  
  Example: `smtp-relay.brevo.com`
- **EMAIL_PORT**: The port for the email server.  
  Example: `587`
- **EMAIL_USERNAME**: The username for authenticating with the email server.  
  Example: `7c6f73001@smtp-brevo.com`
- **EMAIL_PASSWORD**: The password for the email server.  
  Example: `WUa4JBPk8CZybsFr`

---

## Frontend (IGRP UI) Configuration
- **KEYCLOAK_ISSUER**: The Keycloak issuer URL for authentication.  
  Example: `http://${KEYCLOAK_HOSTNAME}:${KEYCLOAK_HOSTNAME_PORT}/realms/${KEYCLOAK_REALM}`
- **IGRP_UI_PORT**: The port where the frontend service runs.  
  Example: `3000`
- **IGRP_UI_HOSTNAME**: The hostname or service name of the frontend service.  
  Example: `igrp-ui`
- **NEXTAUTH_URL**: The URL for NextAuth in the frontend.  
  Example: `http://${IGRP_UI_HOSTNAME}:${IGRP_UI_PORT}`
- **NEXTAUTH_SECRET**: A secret key for NextAuth configuration.  
  Example: `4oC9C+V7ZrANFWiGhcmyvu3GTlOfVDthdxUyn3V3Mtk=`
- **NEXT_PUBLIC_APP_MANAGER_API**: The public URL for the App Manager API.  
  Example: `${USER_MANAGEMENT_URL}`
- **NEXT_PUBLIC_USER_MANAGER_API**: The public URL for the User Management API.  
  Example: `${APP_MANAGER_URL}`
- **APP_MANAGER_API**: The internal URL for the App Manager API.  
  Example: `${APP_MANAGER_URL}`
- **USER_MANAGER_API**: The internal URL for the User Management API.  
  Example: `${USER_MANAGEMENT_URL}`

---

## Environment Files for App Manager and User Management

### `.am.env` - App Manager Configuration
The `.am.env` file provides environment variables specifically for the App Manager service. This file contains the same variables as the main `.env` file, with the exception of Keycloak client credentials, which are unique to the App Manager:

- **KEYCLOAK_CLIENT_ID**: The client ID for the App Manager in Keycloak.  
  Example: `CLIENT_igrp-app-manager`
- **KEYCLOAK_CLIENT_SECRET**: The client secret for the App Manager in Keycloak.  
  Example: `**********`

This file ensures that the App Manager is authenticated as its own client within the Keycloak realm.

---

### `.um.env` - User Management Configuration
The `.um.env` file provides environment variables specifically for the User Management service. Similar to `.am.env`, this file shares the same variables as the main `.env` file, with unique Keycloak client credentials for the User Management service:

- **KEYCLOAK_CLIENT_ID**: The client ID for the User Management service in Keycloak.  
  Example: `CLIENT_igrp-user-manager`
- **KEYCLOAK_CLIENT_SECRET**: The client secret for the User Management service in Keycloak.  
  Example: `**********`

This file ensures that the User Management service is authenticated as a separate client in the Keycloak realm.

---

### Key Notes
- Both `.am.env` and `.um.env` files inherit the same structure as the main `.env` file but customize the **Keycloak client credentials** for their respective services.
- These files allow the App Manager and User Management services to operate independently while sharing the same authentication provider.
- Ensure that the sensitive `KEYCLOAK_CLIENT_SECRET` values are properly secured and not exposed in public repositories.
---

## Minimum requirements (aproximation)

The following requeriments are taken from the environment where this has been tested, therefore use it as an aproximation. Other (or lesser) requirements can be tested under your own risk.

- **Memory**: 8GB RAM 
- **Processor**: 12th Gen Intel Core i5

## How to execute

Before anything, clone the required repositories in the root folder:
1. The User Management API on the `feature/demo-integration` branch:
```
 git clone -b feature/demo-integration http://git.nosi.cv/igrp-3_0/igrp-user-management-api.git
```

2. The App Manager API on the `feature/demo-integration` branch:
```
 git clone -b feature/demo-integration http://git.nosi.cv/igrp-3_0/app-manager-api.git
```
3. The IGRP UI on the `feature/demo-integration` branch:
```
 git clone -b feature/demo-integration http://git.nosi.cv/igrp-3_0/igrp-ui.git
```

This is to have access to all of them. 

Then, simply execute the docker compose file. Notice that Docker must be installed along with the Compose plugin. In case a modification is done to the source code, execute the following command to not use the cache:
```
docker-compose up --force-recreate
```
or 
```
docker compose up --force-recreate
```

Once this is done, both APIs should be ready to use along with a Keycloak realm and a PostgreSQL database. 


## Export/Import a Keycloak realm from a docker container

In case any changes are to be uploaded when initializing the Keycloak Docker, these must be first exported as a json realm. To do so, follow these steps:

### 1. Get the container ID

With the Keycloak container started, obtain its id by executing this command and taking note of the output for the container named `keycloak`
```
docker ps # and take note of the container ID
```

To simplify this step, you can filter the respective container by its name:
```
docker ps --filter "name=keycloak" --format "{{.ID}}"
```

### 2. Get container' bash access
Use this along with the ID retrieved previously
```
docker exec -it [CONTAINER-ID] bash
```

Alternatively, you can access it by executing both commands all together (thus avoiding previous step):
```
docker exec -it $(docker ps --filter "name=keycloak" --format "{{.ID}}") bash
```

### 3. Export the realm 
Once inside the container, get the `quickstart` realm and export it.
```
/opt/keycloak/bin/kc.sh export --dir /opt/keycloak/data/import --users realm_file --realm quickstart --http-management-port 8090
```
> **Note**: If there is a permission error with the folder/file creation for the given realm, it may be due to lack of permissions while replacing a conflicting/existing file. Make sure to delete it if so or change the destination folder in `--dir`.

> **Note 2**: If there is an error with conflicting ports, it may be due to the management interface server conflicting with the container port. If this happens, simply change the port in `--http-management-port`.

### 4. Exit the container
Simply use the exit command
```
exit
```

### 5. Import the realm

Once outside, copy the realm file into the `/data/` folder.

```
docker cp $(docker ps --filter "name=keycloak" --format "{{.ID}}"):/opt/keycloak/data/import/quickstart-realm.json ./data
```


That's it. The new realm should be able after re-executing the docker compose.


