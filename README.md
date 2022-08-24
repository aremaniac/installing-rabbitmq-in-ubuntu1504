# installing-rabbitmq-in-ubuntu1510
Installing rabbitmq in ubuntu 15.10

Here is guide and useful links that may help you when installing RabbitMQ on older version of Ubuntu, especially in Ubuntu 15.10.

As far i know there is no tutorial out there that explaining about how to install RabbitMQ in Ubuntu 15.10, and I decided to use tutorial installation for Ubuntu 18.4.

# Install Erlang Package

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
