---
title: 'NAT:HNS error with Docker SQL Server container on Windows10'
tags:
  - guides
  - How-to
  - howto
  - 'nat: HNS failed'
  - technical
  - Windows
id: '3599'
categories:
  - - Blog
  - - Business Intelligence
  - - Docker
comments: false
date: 2017-08-04 14:29:27
---

[![](http://edpflager.com/wp-content/uploads/2016/05/container_shipping-300x177.png)](http://edpflager.com/?attachment_id=3290#main)As with many things I post here, this article was the result of a problem I encountered, and how to resolve or work around it. I have been working with Docker on Windows and was attempting to run a container provided by Microsoft that included SQL Server. The process is fairly easy, just pull the image and run it with the appropriate command options. Unfortunately, that didn't work. When I used the supplied command to run the container, I would get this error message: Error response from daemon: failed to create endpoint <container name> on network nat: HNS failed with error. Unspecified error. I spent several hours working this out over the past few days, doing web searches and trying various ideas. Lots of people experiencing the problem, but no solutions. Eventually I figured out a solution. Hopefully this helps someone else. The error above I found out indicates a problem with the default 'nat' network that Docker creates when you install it.  By using the default NAT network, you basically have a private network on your docker host. You can attempt to delete that network by opening PowerShell as Administrator. (Microsoft has a good write-up on networking for Windows containers [here](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/container-networking).) Enter the following command to see a list of the virtual networks defined on your PC:

Get-NetNat

Stop Docker using the whale icon on your notification area, by right clicking on the whale and choosing Quit Docker from the menu. Then run this command:

Remove-NetNat

Enter "Y" at the prompt to remove all networks. Now restart Docker.  If this doesn't work for you, read on.
<!-- more -->
So to work around the issue with the default network, we can create a new network for the container to use. The transparent driver type that I specify here starts your container directly connected to the same physical network your host is on. You can assign a static IP address to containers running on this network, or allow DHCP to assign them an IP address. Be aware of the security implications of doing this, including the possibility of malware and viruses. To create the new network, first make sure your Docker environment is setup to run Windows containers and not Linux (the default option). Check this by right clicking the whale icon in your notification panel on the task bar, and looking for this option:[![](http://edpflager.com/wp-content/uploads/2017/08/environment.png)](http://edpflager.com/?attachment_id=3605#main) If it says Switch to Windows containers, select it before you go any further. If it shows like above, you are good to continue. Run this command to create your new network:

docker network create -d transparent <name for network>

Very quickly, you see a long character string that was assigned to your new network. Enter this command and make sure your new network shows up in the list (in my case, the new network is named NEWNET):

docker network ls

[![](http://edpflager.com/wp-content/uploads/2017/08/newnet.png)](http://edpflager.com/?attachment_id=3606#main) Now pull a copy of the container down with:

docker pull microsoft/mssql-server-windows-developer

Then run it with this command to have it use your new network name :

docker run -d --network=transparent -p 1433:1433 -e sa=<your\_sa\_password> -e ACCEPT\_EULA= Y --name=<NET NETWORK NAME>  microsoft/mssql-server-windows-developer

Once it completes starting up, get the container ID with the PS command (It would nice if you could use the name you supplied in the start up command for this next step, but I've had no luck with it):

docker ps -a

[![](http://edpflager.com/wp-content/uploads/2017/08/container_id.png)](http://edpflager.com/?attachment_id=3601#main) With the container ID noted or copied to the clipboard, open up SQL Server Management Studio and input the container ID for the server name, switch Authentication to SQL Server Authentication, enter sa in the Login box, and then enter the password you defined as part of the Docker run command. Click the Connect button, and if all goes well, Management Studio will connect to the SQL Server in your container. [ ![](http://edpflager.com/wp-content/uploads/2017/08/mantstudio.png)](http://edpflager.com/?attachment_id=3602#main) There are additional command switched you can add to make additional databases available outside of your container, but I leave that for you to work out on your own,