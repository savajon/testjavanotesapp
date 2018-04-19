# Jon's Java Note Docker App

## Overview

- Project Contains
-- Dockerfile for Java App
-- Dockerfile for CouchDB instance

## To run

1. Clone this repo
2. Run docker on the compose file to build and run the images
```sh
docker-compose -f docker-compose.yml up -d --build
```
3. Enjoy!
-- By this I mean test via the following
---Java: <ipaddress>:8080/swagger-ui.html
---CouchDB: <ipaddress>:5984/_utils