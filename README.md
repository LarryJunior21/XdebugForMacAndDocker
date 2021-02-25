# Xdebug


## MacOS

In order for Xdebug to work on Docker for MacOS, the container needs a well known IP address
for its Xdebug remote host. To circumvent the Docker installation on MacOS, an additional
alias is required to have a fixed IP address.

This can be either done manually and only persists until a reboot or via a boot persistent
plist script.

### Changing the environment if necessary

Login to your docker machine
#### Example: 
```bash
docker-compose exec --user www-data php74-fpm bash
```
If may vary in your case like
```bash
docker-compose exec --user {user} {your php machine} bash
```
Just replace without the "{}"

#### Edit your docker-php-ext-xdebug.ini file
```bash
vi /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```
#### Add the following
```bash
xdebug.remote_host = 10.254.254.254
xdebug.start_with_request=yes
xdebug.idekey=PHPSTORM
xdebug.discover_client_host=1
```
Then save the file and exit the machine

### Manual
```bash
sudo ifconfig lo0 alias 10.254.254.254
```

#### Check if works
```bash
php -S 10.254.254.254:8080
```

#### Expected Result
```bash
PHP {version}- Development Server started at Thu Feb 25 16:48:15 2021
	Listening on http://10.254.254.254:8080
	Document root is /Users/{user}/Docker
	Press Ctrl-C to quit.
```

### Boot persistent

##### Install
```bash
sudo curl -o \
  /Library/LaunchDaemons/org.xdebug.docker_10254_alias.plist \
  https://raw.githubusercontent.com/LarryJunior21/XdebugForMacAndDocker/main/macos/org.xdebug.docker_10254_alias.plist
```
##### Enable without reboots
```bash
sudo launchctl load /Library/LaunchDaemons/org.xdebug.docker_10254_alias.plist
```

### Credits

https://gist.github.com/ralphschindler/535dc5916ccbd06f53c1b0ee5a868c93
