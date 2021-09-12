# Command test WP-CLI for Vuls

https://github.com/FuCrowRabbit/vuls-proposal-sakura-rental-server/commit/f12b46e6aa284f7bcb683bf55e59f66f82234839

## Warning

This code should be used for testing purposes only.

## Start Docker

```bash
docker-compose up
```

http://localhost:8080/

## Create WordPress database

```bash
docker-compose run --rm wordpress-cli /bin/bash
wp core install --url=example.com --title=Example --admin_user=supervisor --admin_email=info@example.com --prompt=admin_password
```

Set admin password.

> 2/3 [--admin_password=<password>]: PASSWORD

## Run test

### Before

> sudo -u %s -i -- %s theme list --path=%s --format=json --allow-root 2>/dev/null

and

> sudo -u %s -i -- %s plugin list --path=%s --format=json --allow-root 2>/dev/null

#### Bash Result

```bash
docker-compose run --rm wordpress-cli /bin/bash
wp theme list --path=/var/www/html --format=json --allow-root 2>/dev/null
wp plugin list --path=/var/www/html --format=json --allow-root 2>/dev/null
```

##### Output

###### Theme

```json
[{"name":"twentynineteen","status":"inactive","update":"none","version":"2.1"},{"name":"twentytwenty","status":"inactive","update":"none","version":"1.8"},{"name":"twentytwentyone","status":"active","update":"none","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.1.12"},{"name":"hello","status":"inactive","update":"none","version":"1.7.2"}]
```

#### Csh Result

```csh
docker-compose run --user root --rm wordpress-cli /bin/bash
apk add --update tcsh
/bin/tcsh
wp theme list --path=/var/www/html --format=json --allow-root 2>/dev/null
wp plugin list --path=/var/www/html --format=json --allow-root 2>/dev/null
```

##### Output

**Error**

```
Error: Too many positional arguments: 2
```

### After

> sudo -u %s -i -- %s theme list --path=%s --format=json --quiet --allow-root

and

> sudo -u %s -i -- %s plugin list --path=%s --format=json --quiet --allow-root

#### Bash Result

```bash
docker-compose run --rm wordpress-cli /bin/bash
wp theme list --path=/var/www/html --format=json --quiet --allow-root
wp plugin list --path=/var/www/html --format=json --quiet --allow-root
```

##### Output

###### Theme

```json
[{"name":"twentynineteen","status":"inactive","update":"none","version":"2.1"},{"name":"twentytwenty","status":"inactive","update":"none","version":"1.8"},{"name":"twentytwentyone","status":"active","update":"none","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.1.12"},{"name":"hello","status":"inactive","update":"none","version":"1.7.2"}]
```

#### Csh Result

```csh
docker-compose run --user root --rm wordpress-cli /bin/bash
apk add --update tcsh
/bin/tcsh
wp theme list --path=/var/www/html --format=json --quiet --allow-root
wp plugin list --path=/var/www/html --format=json --quiet --allow-root
```

##### Output

###### Theme

```json
[{"name":"twentynineteen","status":"inactive","update":"none","version":"2.1"},{"name":"twentytwenty","status":"inactive","update":"none","version":"1.8"},{"name":"twentytwentyone","status":"active","update":"none","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.1.12"},{"name":"hello","status":"inactive","update":"none","version":"1.7.2"}]
```
