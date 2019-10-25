---
layout: post
title:  "Docker For Devs (Windows)"
date:   2019-10-25 15:42:00 +0100
categories: docker vscode windows
---

### Installation
First install [chocolatey](https://chocolatey.org/install)

Then:
{% highlight powershell %}
choco install docker-for-windows
{% endhighlight %}

install vscode plugin [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

### Run a Dockerfile from vscode
Open "Docker Desktop" from start menu, and wait for the popup "Docker Desktop is now up and running"

In vscode right click Dockerfile -> Build -> hit enter to give it the default name!

In vscode on the Docker plugin tab, right click the image -> run interactive.

### CLI Snippets

Take a look [here](https://github.com/infinityworks/101-Sessions/blob/master/sessions/DockerK8s-100/101/2-Docker/BasicContainers.md) for some fairly decent snippets.

#### _Build a Docker Image_
{% highlight powershell %}
docker build -t image123 .
{% endhighlight %} 

#### _Run a Docker Container, and Open a Shell_
{% highlight powershell %}
docker run -it --name container123 image123
{% endhighlight %} 

#### _Run a Docker Container, Exposed on Port 25000, With An EntryPoint Defined in *.dll, and Delete the Container After Exiting the Shell_
{% highlight powershell %}
docker run -it -d --name container123 -p 25000:80 --entrypoint="dotnet" image123 YourAssemblyWithAnEntryPoint.dll
{% endhighlight %} 

#### _Inspect a Docker Containers' File System That is Already Deployed_
{% highlight powershell %}
# list the running docker containers, look for the {containerid} for your container
docker ps -a -q
# create a snapshot, run it and open a shell
docker commit {containerid} mysnapshot
docker run -t -i mysnapshot /bin/bash
# when finished inspecting, delete the snapshot with:
docker rmi mysnapshot
{% endhighlight %} 

#### _Delete All Docker Images (dev machine only)_
{% highlight powershell %}
docker stop $(docker ps -q)
# a more brutal way of stopped a container than the above
docker kill $(docker ps -q)
docker rmi $(docker images -q)
{% endhighlight %} 

#### _Inspect the entire docker configuration, including port bindings/forwarding_
{% highlight powershell %}
docker inspect {containername}
{% endhighlight %} 