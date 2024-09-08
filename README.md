<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/PhpMyAdmin_logo.svg/2560px-PhpMyAdmin_logo.svg.png" alt="phpMyAdmin Logo" width="600" />
</p>

# Docker Compose Configuration for phpMyAdmin

This document provides a detailed explanation of the Docker Compose configuration used for deploying phpMyAdmin.

## Configuration

```yaml
version: '3.8'  # Docker Compose file format version

services:
  phpmyadmin:
    image: phpmyadmin/phpmyadmin  # Docker image to use for phpMyAdmin
    container_name: phpMyAdmin  # Name of the container for easy management
    restart: unless-stopped  # Container restart policy
    ports:
      - '8084:80'  # Map host port 8084 to container port 80
    environment:
      PMA_HOST: MySQL-Database  # Hostname or network alias of the MySQL container
      PMA_PORT: 3306  # Port number for connecting to MySQL
      PMA_PASSWORD: <Your-MySQL-Password>  # Password for the MySQL root user
    networks:
      - webnet-network  # Network the service will connect to

networks:
  webnet-network:
    external: true  # Use an existing external network
```
# phpMyAdmin and MariaDB Integration

This guide explains how to integrate phpMyAdmin with a MariaDB service using Docker Compose.

## phpMyAdmin Service

### Environment Variables

- **`PMA_HOST`**: Should be set to `MySQL-Database`, which is the `container_name` of the MariaDB service.
- **`PMA_PORT`**: Default port `3306` for MariaDB.
- **`PMA_PASSWORD`**: Should match the `MYSQL_ROOT_PASSWORD` used in the MariaDB service configuration.

### Network

- **`webnet-network`**: Connects phpMyAdmin to the same network as MariaDB, enabling it to communicate with the database.

## Steps to Integrate

1. **Ensure Both Services Are on the Same Network**:
   - Both phpMyAdmin and MariaDB should be connected to the `webnet-network`. This allows phpMyAdmin to discover and connect to the MariaDB container.

2. **Update phpMyAdmin Configuration**:
   - Set `PMA_HOST` in phpMyAdmin to the `container_name` of the MariaDB service (`MySQL-Database`).
   - Use `PMA_PASSWORD` to match the root password defined for the MariaDB service.

3. **Deploy and Access**:
   - Run `docker-compose up` to start both services.
   - Access phpMyAdmin via [http://localhost:8084](http://localhost:8084) (or the port you configured) in your web browser.
   - Log in to phpMyAdmin using the root user credentials defined for MariaDB.

## Summary

- **Networking**: Both services must be on the same Docker network to communicate.
- **Configuration**: Ensure phpMyAdmin is configured with the correct host, port, and password to access MariaDB.
- **Persistence**: The `db_data` volume keeps MariaDB data intact across container restarts.

This setup provides a robust environment where phpMyAdmin can manage your MariaDB databases effectively.



