
# Install and configure a Ubuntu server from scratch

This repo is a group of resources to install and configure a server on an instance of Ubuntu Sever (16.04 tested)

### Access the server
Access the server using ssh
```
$ ssh root@<ip>
```

### Install and configure docker and a limited user
Still with root, download this project
```
# git clone https://github.com/LyseonTech/server-start.git sm
```

#### Make sure about your root password
After the instalation, root will not access ssh anymore. Configure a strong password to your root user. You can generate a password using
```
# sm/password.sh
```

To proceed to update password of root do
```
# passwd
```

> Don't forget of save the password used!

And then you can run the installer like the command above or before execute it read the next topic
```
# sm/ubuntu.sh heimdall 17.12.0~ce-0~ubuntu 1.18.0 yes yes
```

#### What this command will do?
```
# sm/ubuntu.sh <user> <version> <compose> <reboot> <locale>
```

Parameters spec:

1. `<user>` The name of user what will be used to access the server. Can be `sysadmin` or whatever you want. The user what you use to install will be used to next access and root will not access using ssh key anymore;

2. `<version>` [optional] This parameter is the version of docker what will you install. Reproduce the same version of your staging ou test environment. To aplly any version use `edge` or don't inform;

3. `<compose>` [optional] Version of docker-compose. If you use `edge` or don't inform the latest version released will be used;

4. `<reboot>` [optional] Flag for system reboot. If don't informed will be yes;

5. `<locale>` [optional] Fix locales of Ubuntu Server. If don't informed will be yes.

### Managing server

To manage your apps you have the commands:

- `sm add <domain>`: Add one domain to the list of domains

- `sm rm <domain>`: Remove the domain of list of domains

- `sm up`: Make all sites available

- `sm down`: Stop all containers

- `sm enable <domain>`: Enables a specific domain

- `sm disable <domain>`: Disable and stop a specific domain

### Deploying

When you add one app is generated a git repo with a
[docker-compose.yml](https://github.com/LyseonTech/server-manager/blob/master/docker/template/temp/docker-compose.yml)
what needs to be used keeping the lines tagged with `# don't change`.
This repo has a post-receive file what will publish your changes when receive
a push on branch master.

In the deploy process the docker-compose.yml goes down, then, after deploy,
it goes up again. You changes on infra will be applied after finish checkout!

You also can create a volume to map `sh` files to be executed as hooks.
```YAML
    ...
    volumes:
      - ./.hooks/before.sh:/var/www/app/.hooks/before.sh
      - ./.hooks/after.sh:/var/www/app/.hooks/after.sh
    ...
```