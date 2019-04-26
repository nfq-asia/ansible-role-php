# Ansible PHP 

## Description
* The role to install and manage PHP, PHP-FPM, PHP cli, related extensions and tools

## Prerequisites
* An EC2 instance with a static IP mapped to a hostname, running Ubuntu 18.10

## Required variables

* `php_packages` is the list of additional packages to install (through apt):

```yaml
php_packages:
  - "php{{ php_version }}-gd"
  - "php{{ php_version }}-curl"
  - "php{{ php_version }}-json"
  - "php{{ php_version }}-opcache"
  - "php{{ php_version }}-xml"
  - "php{{ php_version }}-mbstring"
  - "php{{ php_version }}-bcmath"
  - "php{{ php_version }}-tidy"
  - "php{{ php_version }}-zip"
  - "php{{ php_version }}-bz2"
  - "php{{ php_version }}-mysql"
  - "php{{ php_version }}-intl"
  - "php{{ php_version }}-imap"
  - "php{{ php_version }}-readline"
  - "php{{ php_version }}-soap"
  - "php-apcu"
  - "php-mongo"
  - "php-mongodb"
  - "php-redis"
  - "php-xdebug"
  - "dh-php"
```

## Defaults
* `php_version`: Default php version is `7.2`.
* `php_log_dir: "/var/log/php"`: Default log path
* `php_fpm_listen: "127.0.0.1:9000"`: Default listening port

PHP-FPM pool parameters defaults are listed here, but should but tuned depending on real usage:

```
php_fpm_max_children: "5"
php_fpm_start_servers: "2"
php_fpm_min_spare_servers: "1"
php_fpm_max_spare_servers: "3"
php_fpm_process_idle_timeout: "10s"
php_fpm_max_requests: "1000"

php_fpm_status_path: "/php_fpm_status"
php_fpm_ping_path: "/php_fpm_ping"

php_fpm_request_slowlog_timeout: "3"

php_fpm_clear_env: "no"

php_fpm_max_execution_time: "60"
php_fpm_max_input_time: "120"
php_fpm_memory_limit: "512M"
php_fpm_post_max_size: "64M"
php_fpm_upload_max_filesize: "64M"
php_fpm_max_file_uploads: "20"
```

## Optional variables

n/a 

## Tags
* `install` - install php services and extensions.
* `configure` - config php services.
## Notes

* The role only defaults to use TCP/IP for FPM traffic
* Support checking php-fpm status by using `/php_fpm_status`:

```
SCRIPT_NAME=/php_fpm_status SCRIPT_FILENAME=/php_fpm_status QUERY_STRING=full REQUEST_METHOD=GET cgi-fcgi -bind -connect 127.0.0.1:9000
```

* Support checking php-fpm ping by using `/php_fpm_ping`

```
SCRIPT_NAME=/php_fpm_ping SCRIPT_FILENAME=/php_fpm_ping REQUEST_METHOD=GET cgi-fcgi -bind -connect 127.0.0.1:9000
```

* This role also support 7.1 and 7.3.
