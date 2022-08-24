# Installing RabbitMQ on Ubuntu 15. 10

Here is guide and useful links that may help you when installing RabbitMQ on older version of Ubuntu, especially in Ubuntu 15.10.

As far i know there is no tutorial out there that explaining about how to install RabbitMQ in Ubuntu 15.10, and I decided to use tutorial installation for Ubuntu 18.4.

I also have tried tutorial for Ubuntu 20.04 and above and it appears did not work, I think because the installation package has too many differences and simple it is not compatible to Ubuntu 15.10.

Ok, lets begin :

** Install Erlang **

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

And it will show something like this

https://user-images.githubusercontent.com/6629806/186396380-ca0bff0d-8fd3-464b-bbe3-5a31c3cf8ab0.png
