[![Build Status](https://travis-ci.org/inmotionhosting/ansible-role-wordpress_ultrastack.png?branch=master)](https://travis-ci.org/inmotionhosting/ansible-role-wordpress_ultrastack) [![GPL-3.0 License](https://img.shields.io/github/license/inmotionhosting/ansible-role-wordpress_ultrastack.svg?color=blue)](https://github.com/inmotionhosting/ansible-role-wordpress_ultrastack/blob/master/LICENSE) [![GitHub stars](https://img.shields.io/github/stars/inmotionhosting/ansible-role-wordpress_ultrastack.svg)](https://github.com/inmotionhosting/ansible-role-wordpress_ultrastack/stargazers)

# Ansible Role: Wordpress UltraStack

Modular Ansible Role for deploying and configuring WordPress featuring InMotion's optimized UltraStack configuration

## Requirements

* CentOS 7.x or later
* Debian 9 or later
* Ubuntu 16.04 LTS or later

## Dependencies

### Required

```yaml
- role: inmotionhosting.apache
- role: inmotionhosting.mysql
- role: inmotionhosting.php_fpm
- role: inmotionhosting.wordpress
- collection: community.general
- collection: ansible.posix
```

### Optional
The following roles are required when `use_ultrastack: true`

```yaml
- role: inmotionhosting.nginx_proxy
- role: inmotionhosting.redis
```

## What is UltraStack?

UltraStack is a set of server configurations created by InMotion Hosting technical staff focused on optimization and performance for specific content management systems.

## What is included?

### NGINX

[NGINX](https://www.nginx.com/resources/wiki/) is part of UltraStack, serving as a reverse-proxy and cache to significantly speed up your website requests. Page cache with a short TTL is utilized to allow efficient handling of a large influx of traffic and cache rules to prevent it from caching logged in/cookie'd users.

### PHP-FPM

[PHP-FPM](https://php-fpm.org/) (FastCGI Process Manager) is an alternative to conventional PHP implementation. Each pool of PHP-FPM works as its own full instance of PHP complete with it's own configs, limits, and resources.

### Redis

[Redis](https://redis.io/), short for Remote Dictionary Server, provides object caching for SQL and other server processes within a database and utilizes much faster system memory instead of using server hard drive resources. This allows for taking those intensive common database queries and caching them which allows for a significantly content delivery.

## Role Variables

Available variables are listed below with their default values (you can also see `defaults/main.yml`)

| Variable | Definition |
| -------- | ---------- |
| use_ultrastack | By default, the inclusion of this role will enable the UltraStack configuration. Switching this to false will prevent the installation of Nginx and Redis.
| use_redis | Enable/disable Redis installation
| site_domain | The domain to associate with service configuration.
| ultrastack_w3tc_settings | If installing on top of WordPress, configure additional W3TC settings.
| nginx_ratelimit_enable | Enable rate limiting on nginx_ratelimit_paths
| nginx_ratelimit_burst | Burst setting on nginx_ratelimit_zone
| nginx_ratelimit_nodelay | Enable or disable Nginx's delay setting on nginx_ratelimit_zone
| nginx_ratelimit_zone | Name of the Nginx rate limit zone
| nginx_ratelimit_paths | Regex paths to rate limit
| nginx_cache_bypass_paths | Regex paths on which to enable cache bypass
| nginx_cache_purge_enable | Switch to enable location block to purge cache using ngx_cache_purge modulle
| nginx_brotli_enable | Switch to enable brotli compression configuration using ngx_brotli module
| nginx_brotli_comp_level | Brotli compression level (1-9)
| nginx_cache_profile | Load a pre-configured NGINX Cache Profile

___Note:___ If using the UltraStack optimizations its highly recommended to use the `w3-total-cache` plugin.

## Example Playbook

```yaml
- hosts: wordpress_ultrastack
  roles:
    - role: inmotionhosting.apache
    - role: inmotionhosting.mysql
    - role: inmotionhosting.php_fpm
    - role: inmotionhosting.wordpress
    # nginx_proxy and redis are included conditionally from within the
    # wordpress_ultrastack role
    - role: inmotionhosting.wordpress_ultrastack
      vars:
        use_ultrastack: true
```

## License

GPLv3

## Author Information

[InMotion Hosting](https://inmotionhosting.com)
