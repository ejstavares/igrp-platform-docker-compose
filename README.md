# APIs_test-environment

Repository to manage and populated a testing environment with all the required connectivity between User Management API, App Manager API, Keycloak and their DB. 

Notice that all the connections and variables are setted in the `.env` file, thereby modifying will have an impact in the whole system.

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
/opt/keycloak/bin/kc.sh export --dir /opt/keycloak/data/import --users realm_file --realm quickstart
```

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


