## Ansible PHP 

### A. Purpose

The task is to write an Ansible role to install and manage PHP, PHP-FPM, PHP cli, related extensions and tools.

  - OS base: Ubuntu 18.04 minimal server with no modifications.
  - PHP: support 7.1, 7.2 and 7.3
  - PHP-FPM: based on php as above
  - PHP extensions: based on php as above

### B. Specifications

1. Input
    - List of related packages/extensions
    - Detail config files for both cli and fpm:
      - Template `php.ini.j2` in this repo for overriding default `/etc/php/7.1/cli/php.ini` file
      - Template `www.conf.j2` in this repo for overiding default `/etc/php/7.1/fpm/pool.d/www.conf` file, also configured for overriding `php.ini` and `php-fpm.conf`

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
          - List of php packages is defined in variable as a list
          - Flexible configure by editing config files on this repo
          - With php cli: does not support ansible variable template for `cli/php.ini`
          - With php fpm: support ansible template, using one config at `fpm/pool.d/www.conf.j2` (configured to override these original configs)
          - `php.ini` and `php-fpm.conf` are still use original configuration files
          - Support checking php-fpm status by using `/php_fpm_status`
          ```
          SCRIPT_NAME=/php_fpm_status SCRIPT_FILENAME=/php_fpm_status QUERY_STRING=full REQUEST_METHOD=GET cgi-fcgi -bind -connect 127.0.0.1:9000
          ```
          - Support checking php-fpm ping by using `/php_fpm_ping`
          ```
          SCRIPT_NAME=/php_fpm_ping SCRIPT_FILENAME=/php_fpm_ping REQUEST_METHOD=GET cgi-fcgi -bind -connect 127.0.0.1:9000
          ```
          - Support php-fpm access log at `/var/log/php/www.access.log`
          - Support php-fpm slow log at `/var/log/php/www.slow.log`
    - **Monitoring**:
      - _default_: discuss in monitoring parts
      - _optional_: discuss in monitoring parts

---
### Defaults
### Required variables
### Optional variables
### Tags
### Notes
