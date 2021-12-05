# Command test WP-CLI for Vuls

https://github.com/FuCrowRabbit/vuls-proposal-sakura-rental-server/commit/f12b46e6aa284f7bcb683bf55e59f66f82234839

## Warning

This code should be used for testing purposes only.

## 1. Start Docker

```bash
docker-compose up
```

http://localhost:8080/

## 2. Create WordPress database

```bash
docker-compose run --rm wordpress-cli /bin/bash
wp core install --url=example.com --title=Example --admin_user=supervisor --admin_email=info@example.com --prompt=admin_password
```

Set admin password.

> 2/3 [--admin_password=<password>]: PASSWORD

## 3. Run test

### Before

> sudo -u %s -i -- %s theme list --path=%s --format=json --allow-root 2>/dev/null

and

> sudo -u %s -i -- %s plugin list --path=%s --format=json --allow-root 2>/dev/null

#### Bash result

```bash
docker-compose run --rm wordpress-cli /bin/bash
wp theme list --path=/var/www/html --format=json --allow-root 2>/dev/null
wp plugin list --path=/var/www/html --format=json --allow-root 2>/dev/null
```

##### Output

###### Theme

```json
[{"name":"twentyfifteen","status":"inactive","update":"available","version":"1.9"},{"name":"twentyseventeen","status":"active","update":"available","version":"1.4"},{"name":"twentysixteen","status":"inactive","update":"available","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.0.1"},{"name":"hello","status":"inactive","update":"available","version":"1.6"},{"name":"wp-multibyte-patch","status":"inactive","update":"none","version":"2.8.1"}]
```

#### Csh result

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

---

### After

> ( sudo -u %s -i -- %s theme list --path=%s --format=json --allow-root > /dev/tty ) >& /dev/null

and

> ( sudo -u %s -i -- %s plugin list --path=%s --format=json --allow-root > /dev/tty ) >& /dev/null

#### Bash result

```bash
docker-compose run --rm wordpress-cli /bin/bash
( wp theme list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
( wp plugin list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
```

##### Output

###### Theme

```json
[{"name":"twentyfifteen","status":"inactive","update":"available","version":"1.9"},{"name":"twentyseventeen","status":"active","update":"available","version":"1.4"},{"name":"twentysixteen","status":"inactive","update":"available","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.0.1"},{"name":"hello","status":"inactive","update":"available","version":"1.6"},{"name":"wp-multibyte-patch","status":"inactive","update":"none","version":"2.8.1"}]
```

#### Csh result

```csh
docker-compose run --user root --rm wordpress-cli /bin/bash
apk add --update tcsh
/bin/tcsh
( wp theme list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
( wp plugin list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
```

##### Output

###### Theme

```json
[{"name":"twentyfifteen","status":"inactive","update":"available","version":"1.9"},{"name":"twentyseventeen","status":"active","update":"available","version":"1.4"},{"name":"twentysixteen","status":"inactive","update":"available","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.0.1"},{"name":"hello","status":"inactive","update":"available","version":"1.6"},{"name":"wp-multibyte-patch","status":"inactive","update":"none","version":"2.8.1"}]
```

#### In the case of Sudo

```csh
docker-compose run --user root --rm wordpress-cli /bin/bash
apk add --update sudo
( sudo -u root -E -- wp theme list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
( sudo -u root -E -- wp plugin list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
apk add --update tcsh
/bin/tcsh
( sudo -u root -E -- wp theme list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
( sudo -u root -E -- wp plugin list --path=/var/www/html --format=json --allow-root > /dev/tty ) >& /dev/null
```

##### Output

###### Theme

```json
[{"name":"twentyfifteen","status":"inactive","update":"available","version":"1.9"},{"name":"twentyseventeen","status":"active","update":"available","version":"1.4"},{"name":"twentysixteen","status":"inactive","update":"available","version":"1.4"}]
```

###### Plugin

```json
[{"name":"akismet","status":"inactive","update":"none","version":"4.0.1"},{"name":"hello","status":"inactive","update":"available","version":"1.6"},{"name":"wp-multibyte-patch","status":"inactive","update":"none","version":"2.8.1"}]
```