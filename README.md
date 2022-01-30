# Quick Docker LEMP Development Stack
Quick development setup for Linux, Nginx, MySQL, PHP, with PHPMyAdmin & Redis


## Requirements
Requires Docker Desktop 3.x or later


## Installation

```
git clone git@github.com:bryaneschultz/lemp-dev-stack.git

cd lemp-dev-stack
```


## Overview
Quick development setup for Linux, Nginx, MySQL, PHP, with PHPMyAdmin & Redis for local development.


## Configuring
Duplicate and rename the .env.example file to .env in the root folder.
Update the values with your own.

In the `src` folder create a `web` folder.
```
mkdir -p [/path/to/project]/src/web
```
Place your public web files in the `src/web` folder.

### Update hosts file
```
echo "127.0.0.1 dev.example.com >> /etc/hosts"
```

### To use with the SSL option (optional)
In the `docker/ssl` folder, create a folder with domain name `example.com`.
```
mkdir -p [/path/to/project]/docker/ssl
```
Place your `fullchain.pem` and `privkey.pem` certs in 
the `docker/ssl/example.com` folder.


## Using Docker LEMP Development Stack
To start on initial run
```
docker-compose up --build
```
or run in detached mode
```
docker-compose up --build -d
```
Subsequent runs
```
docker-compose up
```
or in detached mode
```
docker-compose up -d
```
To stop instance
```
docker-compose down
```


## Roadmap

* Released it

Brought to you by [Bryan E. Schultz](https://www.bryanschultz.net)
