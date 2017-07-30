---
author: slowe
categories: Tutorial
comments: true
date: 2014-07-31T09:00:00Z
slug: installing-the-docker-plugin-for-heat
tags:
- Automation
- Docker
- OpenStack
- Linux
- Virtualization
title: Installing the Docker Plugin for Heat
url: /2014/07/31/installing-the-docker-plugin-for-heat/
wordpress_id: 3480
---

In this post, I'll share with you how I installed the Docker plugin for OpenStack Heat, so that Heat is able to orchestrate the creation of Docker containers in an OpenStack environment. I'm publishing this because I found [the default instructions](https://github.com/openstack/heat/tree/stable/icehouse/contrib/docker/docker) to be a bit too vague to be helpful. By sharing my experience, I hope that others interested in using Docker in their OpenStack environment will benefit.

Here are the steps I used to make the Docker plugin work with Heat. These steps assume you are using Ubuntu and already have OpenStack Heat installed and working correctly:

1. If you are using the packaged version of Heat (in other words, you are installing Heat via a method like `apt-get install` on Ubuntu), then you'll want to use the "stable/icehouse" branch that contains the Docker container. In this case, you _don't_ want to use master---it won't work (either the plugin won't load or the Heat engine service won't start). Download a ZIP copy of the correct branch of Heat from GitHub (for "stable/icehouse", see [here](https://github.com/openstack/heat/tree/stable/icehouse)).

2. Extract the `contrib/docker` folder from the downloaded ZIP copy of Heat.

3. Delete the `contrib/docker/docker/tests` directory; in my testing, the plugin failed to load if you leave this directory present in the plugin.

4. Copy the `contrib/docker` folder to your OpenStack controller somewhere. On my controller, I chose to put it into an existing `/var/lib/heat` directory. When you're done, you should have a `docker` directory in your chosen destination, and that directory should container another subdirectory named `docker`. For example, on my system, the full path to the plugin was `/var/lib/heat/docker/docker`. Make note of the full path.

5. In the top-level `docker` folder, run `pip install -r requirements.txt`. Note that you might need to do an `apt-get install python-pip` first. This will install the docker-py Python module, which is required by the Docker plugin.

6. Modify your Heat configuration file (typically found at `/etc/heat/heat.conf`) and add the full path of the Docker plugin to the `plugin_dirs` setting. If you used `/var/lib/heat` as the base directory for the plugin, then the full path should be `/var/lib/heat/docker/docker`.

7. Restart the Heat engine (via something like `sudo service heat-engine restart` or similar).

8. Run `heat resource-type-list` and verify that DockerInc::Docker::Container is listed in the results. If not, verify that you have the correct path to the plugin specified in the Heat configuration file, and verify that you used the correct branch of the Docker plugin ("stable/icehouse" if you are using packaged versions of OpenStack). Review the Heat log files for any errors if the resource type still isn't listed.

Assuming you were successful, then you are ready to start deploying Docker containers via Heat. Stay tuned for an example Heat template that shows how to deploy a Docker container. Until then, feel free to share any corrections, clarifications, or questions in the comments below.
