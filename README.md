# Moodle
### Install Moodle with databases MariaDB and PhpMyAdmin

```bash
version: '3'

services:
  db:
    image: docker.io/bitnami/mariadb:10.5
    container_name: db
    restart: always
    user: root
    privileged: yes
    environment:
      MARIADB_USER: bn_moodle
      MARIADB_PASSWORD: bitnami
      MARIADB_DATABASE: bitnami_moodle
      MARIADB_CHARACTER_SET: utf8mb4
      MARIADB_COLLATE: utf8mb4_unicode_ci
      MARIADB_ROOT_PASSWORD: root
    ports:
      - 3306:3306
    volumes:
      - $PWD/mariadb:/bitnami/mariadb

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: pma
    restart: always
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
    ports:
      - 8081:80

  moodle:
    image: bitnami/moodle
    container_name: moodle
    restart: always
    environment:
      MOODLE_USERNAME: admin
      MOODLE_PASSWORD: password
      MOODLE_EMAIL: correo.interno@codesa.com.co
      MOODLE_DATABASE_HOST: db
      MOODLE_DATABASE_PORT_NUMBER: 3306
      MOODLE_DATABASE_USER: bn_moodle
      MOODLE_DATABASE_NAME: bitnami_moodle
      MOODLE_DATABASE_PASSWORD: bitnami

    ports:
      - 80:8080
      - 443:8443
    volumes:
      - $PWD/moodle_data:/bitnami/moodle
      - $PWD/moodledata_data:/bitnami/moodledata
    depends_on:
      - db
 
volumes:
  mariadb:
  moodle_data:
  moodledata_data:

```
