+++
title = "Grafana in docker setup"
date = 2018-09-06T11:54:05+02:00
tags = [""]
categories = [""]
draft = false
+++


# Grafana

Grafana is a tool that lets you visualize metrics. It handles a lot of different data sources and is very flexible. It does not require you to be an it expert to setup and with just few easy steps you can connect to your database or srvice and present live metric that can help you more deeply understand how you system is used. With a system of alerts it tell you about bottlenecks that you might not know of, like moments in time when your system is overloaded and you database is dying.

And it is all free and open sourced so 😍. 

# What for

Today most multi-services platforms are build using docker containers. It provides you with great flexibility and this is what I wanted to have with grafana just a standalone docker image that I can have preconfigured and ready to deploy in dc/os for example.

What I wanted to achieve with grafana is to have a nice display of what is happening in production database of one of the companies I work for. I wanted to create dashboard that will query some data and will present valuable information that we can easily interpret. 

So What I am going to explain here is a grafana connection to mysql database. But as I an imagine other data sources might by as "simple" as this one. After fighting all issues that I encountered everything else should be easy.

# Initial setup

So lets begin by making sure that you have docker installed. I used the latest version that is available [here](https://docker.com). I will be using terminal commands a lot so just be sure to keep terminal window opened.

After that we need to have grafana docker image installed. It is easily available from docker hub (it is where all official docker images are available). So to download grafana image just type in:

`docker pull grafana/grafana`

This will download grafana image, to run it and make sure that everything is working correctly just type in terminal:

`docker run -d grafana/grafana`

The `-d` option here is to run image as a demon. Grafna is configured to run on port `3000`, it is a popular port so before running make sure it is not taken already on your machine. If you already have something runing there then it is easy to change port for docker container, just change above command like this:

`docker run -d -p 3001:3000 grafana/grafana`

it will start on `3001` port, you get the idea how it works, `3001` is a external port and `3000` is a docker inside port that will be mapped to external. Now that everything is running you just need to check if it is working just by going to:

`localhost:3000` 

It should display something like this:

![grafana config](/static/gf_config.png)

# Configuring docker image

Well as you probably already know, there are multiple options to change container behavior. What I did is prepared my own grafana docker image. This way I am able to configure everything I want, build image and run it.

## Creating grafana docker image

First we need to create `Dockerfile` yes, there is no extension there. 
I will not describe what parameters you can add into dockerfile cause there are so many, I will just describe the one I used.

So here is all that you need to have in your dockerfile to be able to build accessible grafana image:


```
FROM grafana/grafana

ENV GF_AUTH_DISABLE_LOGIN_FORM "true"
ENV GF_AUTH_ANONYMOUS_ENABLED "true"
ENV GF_AUTH_ANONYMOUS_ORG_ROLE "Admin"
```

If you have it already then the only thing to do is call docker build in terminal like this:

`docker build -t my-grafana .`

And that is it, it will build fully workable docker container with grafana inside.

Let me now describe what is going on in this file.

`FROM` keyword describes what source image will be used to build this one, in our case it will be default `grafana/grafana` image.
Next is `ENV` keyword that is responsible for adding new env variables to the container, so when it starts it already has these variables set.

### Grafana configuration

So Why I am using exactly these variables. Grafana by default is configured with `grafana.ini` file that is located in `/etc/grafana/grafana.ini` or `/usr/local/etc/grafana/grafana.ini`. This file contains a lot of configuration possibilities but there is no way to modify them inside container because when we run container is is to late to go and modify the file, server is already up and ini file is already loaded.
But to overcome this limitation, grafana introduced env variables convention. It basically matches initial configurations with corresponding environment variable.
To create correct variable we need to know what name it has in `grafana.ini` file.
So here are some examples of variable names and their matches from file:

```
    GF_AUTH_DISABLE_LOGIN_FORM "true" = 
    [auth]
    disable_login_form = true
    ...
    GF_AUTH_ANONYMOUS_ENABLED "true" = 
    [auth.anonymous]
    enabled = true
```

I think you can get the idea. Every variable must start with `GF_` and then goes section title plus underscore, if section title contain dot then we can replace dot with underscore, at the end upper cased configuration name and value.

### Content configuration

So for my case I wanted to already have some content inside my server, like default data source or some initial dashboard. This way I can always put these files in repository and track changes.

So how can we do than. Well fortunately grafana supports that kind of stuff as well our of the box, amazing 💯.

### Adding data source

So to add new data source we need to create configuration file called `datasources.yaml` inside container under `/etc/grafana/provisioning/datasources`.
Again the problem is that it is in container and after container is running it is to late. So what we can to is mount a volume under that path.

So first we need to create folder `provisioning` and inside folder named `datasources` and then inside this folder create file datasources.yaml that will contain our default datasource configuration.
But what kind of configuration we can put there, where besides looking into grafana documentation you can go into container and look into sample configuration file. First display all running containers with command

`docker ps`

it should aoutput something like this:

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
8ecddfc1ade3        my-grafana          "/run.sh"           About an hour ago   Up About an hour    0.0.0.0:3000->3000/tcp   elastic_brown
```

What's important is container id so copy that and execut next command

`docker exec -it container_id bash`

This command will take you inside the *running* container. Here you have a bare linux image. So to display sample datasource file configuration just run

`cat ./usr/share/grafana/conf/provisioning/datasources/sample.yaml`

This should display a sample, something like this:

```
# # config file version
apiVersion: 1

# # list of datasources that should be deleted from the database
#deleteDatasources:
#   - name: Graphite
#     orgId: 1

# # list of datasources to insert/update depending
# # on what's available in the datbase
#datasources:
#   # <string, required> name of the datasource. Required
# - name: Graphite
#   # <string, required> datasource type. Required
#   type: graphite
#   # <string, required> access mode. direct or proxy. Required
#   access: proxy
...
```

As you can se there are a lot options to play with, bot for now what is important for us is section where datasources are described.

**Remember that yaml format is very sensitive to any kind of empty signs, additional signs**

So first configuration should look something like this:

{{< highlight yaml "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
datasources:
    - name: default
      type: mysql
      orgId: 1
      url: http://localhost:3306
      password: ""
      user: root
      database: test
      editable: true
{{< / highlight >}}

Save this file inside created directory structure.
Now to mount this as a volume we need to execute `docker run` with additional `-v` parameter. Cd to root path where `provisiong` dir is. 
Remember that we want to replace path inside container `/etc/grafana/provisioning/datasources` with `./provisioning` to so to mount that dir just call

`docker run -p 3000:3000 -d -v $(pwd)/provisioning:/etc/grafana/provisioning my-grafana`

Now if you go to `localhost:3000` under Configuration -> Data Source you can see that our test configuration is added.

[img src here]


### Adding dashboard






