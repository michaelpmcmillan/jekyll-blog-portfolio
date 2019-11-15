---
layout: post
title:  "Monitor a dotnet core web-api with newrelic in docker (linux)"
date:   2019-11-15 09:10:00 +0100
categories: docker newrelic dotnet core
---

### Prerequisites
docker
vscode
vscode plugin: [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
already have created a dotnet core application
already have a `Dockerfile`

### Add the newrelic agent to the docker image
Add these sections to your `Dockerfile`, directly after the `FROM base AS final`.

#### Install the agent
{% highlight powershell %}
RUN apt-get update && apt-get install -y wget ca-certificates gnupg \
&& echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
&& wget https://download.newrelic.com/548C16BF.gpg \
&& apt-key add 548C16BF.gpg \
&& apt-get update \
&& apt-get install -y newrelic-netcore20-agent
{% endhighlight %}

#### Enable the Nuget agent
{% highlight powershell %}
ENV CORECLR_ENABLE_PROFILING=1 \
CORECLR_PROFILER={36032161-FFC0-4B61-B559-F6C5D41BAE5A} \
CORECLR_NEWRELIC_HOME=/usr/local/newrelic-netcore20-agent \
CORECLR_PROFILER_PATH=/usr/local/newrelic-netcore20-agent/libNewRelicProfiler.so \
NEW_RELIC_LICENSE_KEY=REPLACE_THIS_WITH_YOUR_NEWRELIC_LICENCE_KEY \
NEW_RELIC_APP_NAME='REPLACE THIS WITH YOUR APPLICATION NAME'
{% endhighlight %}

### Test
Build and run your docker image using terminal or the vscode [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) plugin.

Load up your web-api and hit an endpoint.  It takes a minute or so for the logs to be sent to NewRelic's servers and to appear in your charts.

![NewRelic](/assets/newrelic.jpg)