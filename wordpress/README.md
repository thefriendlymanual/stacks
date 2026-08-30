# WordPress

WordPress is the World's most popular contnant management system which still powers a large percentage of the world's websites.

This stack includes WordPress, Redis object cache and MariaDB

After bringing the stack up, browse to your URL and complete the WordPress installation. 

Then install and activate a Redis object-cache plugin from wp-admin, e.g.
   **Redis Object Cache** (by Till Krüss). It will detect the
   `WP_REDIS_HOST` / `WP_REDIS_PORT` constants already defined via
   `WORDPRESS_CONFIG_EXTRA` and just needs "Enable Object Cache" clicked
   on its settings page (Settings → Redis)

   Confirm the object-cache is working: Settings → Redis in wp-admin should show
   "Status: Connected"

## Updating the stack

Always back up or take a snapshot of your wp_data volume and you wp database before upgrading

Docker best practice in production is to pin containers to specific versions. Change the version in compose.yaml then pull and re-deploy the stack to upgrade.

In the case of the WordPress container, all versiom, theme and plugin updates executed through the wp-admin console are persisted in the wp_data volume. 
So in many use cases, leaving the wp container tag as 'latest' and upgrading via wp-admin will work fine. Periodically pull and redploy the wp container (or the stack) to keep the app container up to date (OS, Apache, php etc.)

You can still pin the version of the wp container if it suits you

## References

[Website](https://wordpress.org/)  
[Docker Hub](https://hub.docker.com/_/wordpress)  
[Documentation](https://wordpress.org/documentation/)  