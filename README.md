# Challenge

## Overview

This project provides a Docker Compose setup for deploying a Laravel API backend, Nuxt.js frontend, and a MySQL database. It leverages a custom network for inter-service communication and offers optional integration with phpMyAdmin for database management.

## Project Structure
.
├── README.md  (This file)
├── api/
│   ├── Dockerfile
│   ├── ... (Laravel API code and configuration)
├── client/
│   ├── Dockerfile
│   ├── ... (Nuxt.js frontend code and configuration)
└── docker-compose.yml  (This configuration file)

## Table of Contents

-Getting Started
-Services
-Nginx
-PHP
-MySQL
-phpMyAdmin
-Nuxt.js
-Volumes
-Networks
-Useful Commands

## Services
Nginx
Image: nginx
Container Name: nginx
Ports: 7000 (host) -> 80 (container)
Volumes:
./api/nginx/default.conf:/etc/nginx/conf.d/default.conf
./api:/var/www/app:delegated
Dependencies: Depends on PHP service
------------------------------------------------------------
PHP
Build Context: ./api
Dockerfile: ./api/Dockerfile
Container Name: php
Expose: 9000 (for inter-service communication)
Volumes: ./api:/var/www/app:delegated
Networks: Connects to laravel network
------------------------------------------------------------
MySQL
Image: mysql:5.7
Container Name: mysql
Environment Variables:
MYSQL_ROOT_PASSWORD: rootpassword
MYSQL_DATABASE: bookapi
MYSQL_USER: app
MYSQL_PASSWORD: password
Volumes: mysql_data:/var/lib/mysql
------------------------------------------------------------
phpMyAdmin
Image: phpmyadmin/phpmyadmin
Container Name: phpmyadmin
Ports: 8081 (host) -> 80 (container)
Environment Variables:
PMA_HOST: mysql
MYSQL_ROOT_PASSWORD: rootpassword
Dependencies: Depends on MySQL service
------------------------------------------------------------
Nuxt.js
Build Context: ./client
Dockerfile: ./client/Dockerfile
Container Name: nuxt-app
Ports: 3000 (host) -> 3000 (container)
Environment Variables: NODE_ENV=production
Volumes: ./client:/app
Command: npm run serve
Volumes
mysql_data: Used for persisting MySQL data. Uses the local driver.