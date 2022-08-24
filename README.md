# Installing RabbitMQ on Ubuntu 15. 10

Here is guide and useful links that may help you when installing RabbitMQ on older version of Ubuntu, especially in Ubuntu 15.10.

As far i know there is no tutorial out there that explaining about how to install RabbitMQ in Ubuntu 15.10, and I decided to use tutorial installation for Ubuntu 18.04.

I also have tried tutorial for Ubuntu 20.04 and above and it appears did not work, I think because the installation package has too many differences and simply it is not compatible to Ubuntu 15.10.

Ok, let's begin :

**Installing Erlang**

Add the following source list : 

```
echo "deb https://packages.erlang-solutions.com/ubuntu bionic contrib" | sudo tee /etc/apt/sources.list.d/erlang-solution.list
```

And then update the local package cache with the newly added repository : 

```
sudo apt update
```

Install erlang :

```
sudo apt install erlang
```

Once the installation finished, you can test it out with this command :

```
erl
```

And it will shows something like this:

![image](https://user-images.githubusercontent.com/6629806/186396380-ca0bff0d-8fd3-464b-bbe3-5a31c3cf8ab0.png)


**Preparation**

Before we installing RabbitMQ, lets make sure configuration of your host is correct, because it can cause an error if something missing.

Type `hostname` in your terminal, you'll see your hostname, after that edit your host and make sure the IP Address and the hostname are correctly mapped. Edit your host file with nano editor :

 ```
 nano /etc/hosts
 ```
Here is my configuration looks like :
![image](https://user-images.githubusercontent.com/6629806/186467800-ebaeff1d-b8e4-45d3-beed-acad4b4c7613.png)


**Installing RabbitMQ**

And from now on, I'll follow instruction from RabbitMQ official page to install the RabbitMQ package on Ubuntu 18.04 (you can see the tutorial here [*Using RabbitMQ Apt Repositories on Cloudsmith*](https://www.rabbitmq.com/install-debian.html#apt-cloudsmith)). 

I have tried so many other tutorials and just end up with errors. One of the reason is the repo for the packages is already dead, since the version of Ubuntu it self is no longer supported.

Following the instruction below i also head up with some errors, but I'll mention it later how to deal with it, in case you face it too.

Let's start with installing this package :
```
sudo apt-get install curl gnupg apt-transport-https -y
```

Team RabbitMQ's main signing key :
```
curl -1sLf "https://keys.openpgp.org/vks/v1/by-fingerprint/0A9AF2115F4687BD29803A206B73A36E6026DFCA" | sudo gpg --dearmor | sudo tee /usr/share/keyrings/com.rabbitmq.team.gpg > /dev/null
```

If you follow the instruction in RabbitMQ page, you'll find the steps of adding key and repo for erlang, but we will skip it, because we have already install another version of erlang. So just move to another step.

Cloudsmith: RabbitMQ repository : 
```
curl -1sLf https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/gpg.9F4587F226208342.key | sudo gpg --dearmor | sudo tee /usr/share/keyrings/io.cloudsmith.rabbitmq.9F4587F226208342.gpg > /dev/null
```

Add apt repositories maintained by Team RabbitMQ : 
```
sudo tee /etc/apt/sources.list.d/rabbitmq.list <<EOF
## Provides RabbitMQ
##
deb [signed-by=/usr/share/keyrings/io.cloudsmith.rabbitmq.9F4587F226208342.gpg] https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/deb/ubuntu bionic main
deb-src [signed-by=/usr/share/keyrings/io.cloudsmith.rabbitmq.9F4587F226208342.gpg] https://dl.cloudsmith.io/public/rabbitmq/rabbitmq-server/deb/ubuntu bionic main
EOF
```

Update package indices :
```
sudo apt-get update -y
```

Install rabbitmq-server and its dependencies : 
```
sudo apt-get install rabbitmq-server -y --fix-missing
```

Wait until the installation finished.

Check the status of RabbitMQ :
```
status rabbitmq-server.service
```

Check if RabbitMQ is enabled :
```
systemctl is-enabled rabbitmq-server.service
```

To start and stop the server, use the `systemctl` tool. The service name is `rabbitmq-server`:
```
# stop the local node
sudo systemctl stop rabbitmq-server

# start it back
sudo systemctl start rabbitmq-server
```

`systemctl status rabbitmq-server` will report service status as observed by systemd (or similar service manager):
```
# check on service status as observed by service manager
sudo systemctl status rabbitmq-server
```

It will produce output similar to this:
```
Redirecting to /bin/systemctl status rabbitmq-server.service
● rabbitmq-server.service - RabbitMQ broker
   Loaded: loaded (/usr/lib/systemd/system/rabbitmq-server.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/rabbitmq-server.service.d
           └─limits.conf
   Active: active (running) since Wed 2021-05-07 10:21:32 UTC; 25s ago
 Main PID: 957 (beam.smp)
   Status: "Initialized"
   CGroup: /system.slice/rabbitmq-server.service
           ├─ 957 /usr/lib/erlang/erts-10.2/bin/beam.smp -W w -A 64 -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -K true -- -root /usr/lib/erlang -progname erl -- -home /var/lib/rabbitmq -- ...
           ├─1411 /usr/lib/erlang/erts-10.2/bin/epmd -daemon
           ├─1605 erl_child_setup 400000
           ├─2860 inet_gethost 4
           └─2861 inet_gethost 4

Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##  ##
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##  ##      RabbitMQ 3.8.17. Copyright (c) 2007-2022 VMware, Inc. or its affiliates.
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##########  Licensed under the MPL 2.0. Website: https://www.rabbitmq.com/
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ######  ##
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##########  Logs: /var/log/rabbitmq/rabbit@localhost.log
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: /var/log/rabbitmq/rabbit@localhost_upgrade.log
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: Starting broker...
Dec 26 10:21:32 localhost.localdomain rabbitmq-server[957]: systemd unit for activation check: "rabbitmq-server.service"
Dec 26 10:21:32 localhost.localdomain systemd[1]: Started RabbitMQ broker.
Dec 26 10:21:32 localhost.localdomain rabbitmq-server[957]: completed with 6 plugins.
```

**Enable the RabbitMQ Management Dashboard (Optional)**
You can optionally enable the RabbitMQ Management Web dashboard for easy management.
```
sudo rabbitmq-plugins enable rabbitmq_management
```

The Web service should be listening on TCP port `15672`
```
$ sudo ss -tunelp | grep 15672
```

If you have an active UFW firewall, open both ports 5672 and 15672:
```
sudo ufw allow proto tcp from any to any port 5672,15672
```

Access it by opening the URL `http://[server IP|Hostname]:15672`

By default, the guest user exists and can connect only from localhost. You can login with this user locally with the password “guest”

To be able to login on the network, create an admin user like below:
```
sudo rabbitmqctl add_user admin StrongPassword
sudo rabbitmqctl set_user_tags admin administrator
```

That's up the tutorial is finished. 

As for the addition, I'll mention some problem solving to errors that you have encounters, here it is:

If you failed when adding the repo keys with the above instruction, you can instead downloading it and import it manually. First copy-paste the key URL to your browser, and you will see some text that contain the key. Save it to a file (for example I name it `keyfile1`) and then import it with this command :
```
sudo apt-key add keyfile1
```

In case you uncorrectly adding the wrong key, you can remove it by this command :
```
sudo apt-key del THE_HEXADECIMAL_KEY
```

To identify which key you want to select, use this command first :
```
sudo apt-key list
```
