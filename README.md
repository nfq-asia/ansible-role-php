## Ansible PHP 

### A. Purpose

The task is to write an Ansible role to install and manage PHP, PHP-FPM, PHP cli, related extensions and tools.

  - OS base:  Ubuntu 18.04 minimal server with no modifications.
  - PHP: 7.2 (latest version)
  - PHP-FPM: 7.2 (based on php as above)
  - PHP extensions (based on php as above)
  - Composer: 1.8.4 (latest version)

### B. Specifications

1. Input
    - List of related packages/extensions
    - Detail config files for both cli and fpm:
      - Minimal version of `php.ini` in this repo for overriding default `/etc/php/7.1/fpm/php.ini` file and `/etc/php/7.1/cli/php.ini` file
      - Minimal version of `www.conf` in this repo for overiding default `/etc/php/7.1/fpm/pool.d/www.conf` file

2. Prerequisites
    - An EC2 instance with a static IP mapped to a hostname

3. Output
    - **Connectivity**:
      - _external_: no external connection
      - _internal_: within AWS private subnet:
          - other services like nginx will connect to PHP-FPM via `127.0.0.1:9000` as defaut
          - not support PHP-FPM socket type
          - can flexible run PHP cli `bin/console` command inside this VM
    - **Configuration**:
      - _default_:
          - all default configurations as the original of php7.2 & php-fpm but minimal version (removed some useless comment lines)
          - Running by user `www-data` as default
          - internal connection via `127.0.0.1:9000` as default
      - _advanced_:
          - Flexible configure by editing config files on this repo
          - `php.ini` and `www.conf` are pure configuration file
          - Does not support ansible variable template (because we have over 100 LoC for each config) 
    - **Monitoring**:
      - _default_: discuss in monitoring parts
      - _optional_: discuss in monitoring parts

---
### Defaults
### Required variables
### Optional variables
### Tags
### Notes
