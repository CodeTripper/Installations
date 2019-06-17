# Installations
## KONG
### using docker
   DB less mode
   
    docker network create kong-net
    
   Create a volume in the host machine
   
    docker volume create kong-vol
    
   Inspect the volume
   
    docker volume inspect kong-vol
 
    [
     {
         "CreatedAt": "2019-05-28T12:40:09Z",
         "Driver": "local",
         "Labels": {},
         "Mountpoint": "/var/lib/docker/volumes/kong-vol/_data",
         "Name": "kong-vol",
         "Options": {},
         "Scope": "local"
     }
    ]
 
 
  Create a configuration file called kong.yml  ans ave it inside the mountpoint above.
  Start kong to listen for traffic at 8000/8443 and for admin at 8001/8444
  
     docker run -d --name kong \
     --network=kong-net \
     -v "kong-vol:/usr/local/kong/declarative"
     -e "KONG_DATABASE=off" \
     -e "KONG_DECLARATIVE_CONFIG=/usr/local/kong/declarative/kong.yml" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     kong:latest
