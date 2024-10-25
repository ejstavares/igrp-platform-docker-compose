# APIs_test-environment

Repository to manage and populated a testing environment with all the required connectivity between User Management API, App Manager API, Keycloak and their DB. 

Notice that all the connections and variables are setted in the `.env` file, thereby modifying will have an impact in the whole system.

## How to execute

Before anything, clone both the User Management API and the App Manager API in the root folder. This is to have access to both of them. 

Then, it is imperative to specify the machine hostname. This is due to the need of having an equal hostname between internal containers and the external host. To do so, use the following command (in Linux based environments) to obtain the `hostname` and override it in the `.env` file:
```
sed -i~ "/^KEYCLOAK_HOSTNAME=/s/=.*/=$(hostname)/" .env
```


Finally, simply execute the docker compose file. Notice that Docker must be installed along with the Compose plugin. In case a modification is done to the source code, execute the following command to not use the cache:
```
docker-compose up --force-recreate
```
or 
```
docker compose up --force-recreate
```


Once this is done, both APIs should be ready to use along with a Keycloak realm and a PostgreSQL database. 
