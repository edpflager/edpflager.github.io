---
title: Map Windows host location to Linux container in Docker
tags:
  - external article
  - How-to
  - howto
  - Linux
  - nginx
  - technical
  - Windows
id: '3628'
categories:
  - - Blog
  - - Docker
comments: false
date: 2017-10-04 14:05:15
---

[![](http://edpflager.com/wp-content/uploads/2017/10/map-300x225.jpg)](http://edpflager.com/?attachment_id=3640#main)I've been working more with Docker on a Windows PC lately. With the more recent versions of Docker, the application runs much better and there is task bar control panel for managing the processes. If you are interested in trying out Docker and don't have a Linux machine to work with, go download the Windows [Stable Community edition](https://store.docker.com/editions/community/docker-ce-desktop-windows) which was recently updated to 17.09.0.
<!-- more -->
This post is the result of something I have been trying to find solve for a while. If you are running an all Linux Docker ecosystem, mapping a volume from host to container is very quick and easy. Just include the volume command line switch: -v <host location>:<container location>. So for example, if you were starting an NGINX webserver container, you could create a directory under your home location called **~/nginx/web**  where you can put HTML files. Then when you start the NGINX container, you add a volume map switch and point the NGINX container location for HTML to your local directory like this:

```
-v ~/nginx/web:/usr/share/nginx/html
```

If you place an INDEX.HTML file in that folder, NGINX will serve that page up rather than the default page it normally does. [![](http://edpflager.com/wp-content/uploads/2017/10/SharedDrives-300x196.png)](http://edpflager.com/?attachment_id=3637#main)But on Docker for Windows, its not quite that easy. First in the Docker Control Panel, you need to enable local access to Docker. Right click the Docker whale icon, and choose settings from the menu that appears. The Settings window will appear, and you will need to select the Shared Drives option on the left. In the list of Drives check the ones you want to share in Docker. Click the Apply button at the bottom, and you will be prompted for your credentials to make sure you have authority to allow this. If you are working on a corporate domain and are using system that may be used away from the domain (like a laptop), you may want to setup a local system account that has permission to do this, and use that account. Now once you have shared the drive you want Docker containers to have access to, you still need to deal with the odd notation Docker uses in Windows  to map the drive. Let me illustrate with an example. From the root of your Windows C: drive (or another drive if one is available), create a folder called nginx, and then a subfolder called web. Add a simple index.html file to that folder. Doesn't need to be much beyond a Hello World type entry, like this

<html>
  <head>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" integrity="sha256-MfvZlkHCEqatNoGiOXveE8FIwMzZg4W85qfrfIFBfYc= sha512-dTfge/zgoMYpP7QbHy4gWMEGsbsdZeCXz7irItjcC3sPUFtf0kuFbDz/ixG7ArTxmDjLXDmezHubeNikyKGVyQ==" crossorigin="anonymous">
    <title>Docker NGINX Sample page</title>
  </head>
  <body>
    <div class="container">
      <h1>Hello! This page brought to you by Docker! </h1>
    </div>
   </body>
</html>

Finally, start your NGINX container with this volume map switch: -v /c/nginx/web:/usr/share/nginx/html So the complete Docker RUN command would be (updated) :

docker run --name NginxTest -d nginx --rm -p 80:80 -v /c/nginx/web:/usr/share/nginx/html

(that's two hyphens before name, and rm) Open a web browser and go to http://localhost and you should see the NGINX website, with your simple Hello World page!  Follow the same notation whenever you want to map a Windows folder to a Linux container.