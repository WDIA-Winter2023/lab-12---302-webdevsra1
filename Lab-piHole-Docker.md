# Lab piHole

This lab is adapted from https://pimylifeup.com/pi-hole-docker/

This tutorial will show you how to install and run Pi-Hole as a Docker container.

![Pi-Hole Docker Container](https://cdn.pimylifeup.com/wp-content/uploads/2022/10/Pi-hole-docker-container-Thumbnail.jpg)

Pi-Hole is a software that act as your DNS provider to actively block internet ads and trackers. It does this by filtering the DNS requests and sending any blocked domains into a blackhole, so the request is never completed.

Another cool thing about Pi-Hole is that it can also work well to monitor your network traffic, as you can set it to log any DNS requests that the server receives.

You can run Pi-Hole on your devices in many ways, but one of the easiest is to use Docker. The advantage of using Docker is that everything you need is set up within the container.

## Preparing your System to Run Pi-Hole as a Container

**1.** As usual it is a good idea to update your Pi

```bash
sudo apt update
```

**2.** This was done last week so you may not need to do it.

```bash
sudo apt install docker-compose
```

## Installing the Pi-Hole Docker Container

This section will show you the process of installing Pi-Hole as a Docker container on your Linux-based system. All we need to do within this section is to write a “`docker-compose`” configuration file.

This file tells Docker what containers it needs to download and what ports it needs to open.

### Creating a Directory for Pi-Hole

**1.** Start by creating a directory where you will store the configuration file for the Pi-Hole docker container.

```bash
mkdir ~/pihole
```

**2.** Let us move into the newly created directory

```bash
cd ~/pihole
```

### Writing the Docker-Compose Configuration File

**3.** Our next step is writing the “`docker-compose.yml`” file. This file is where we will define the Pi-Hole docker container and the options we want passed to the container.

```bash
nano docker-compose.yml
```

**4.** Within this file, you will want to enter the following lines. We will explain the pieces you may want to modify shortly.

```yaml
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
    environment:
      TZ: 'America/Montreal'
      WEBPASSWORD: 'set a secure password here or it will be random'
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```



### Configuring the Pi-Hole Configuration File

**5.** Before you save this file, there are three Docker options that you will want to reconfigure for Pi-Hole to suit your setup better.

#### Setting the Password for the Pi-Hole Web Interface

Begin by looking for the following line within the configuration file.

```yaml
      # WEBPASSWORD: 'set a secure password here or it will be random'
```

Replace with the following, switching out “`SECUREPASSWORD`” with a secure password of your own. Try and use letters, numbers, and symbols.

```yaml
      WEBPASSWORD: 'SECUREPASSWORD'
```

#### **Configuring the Web Interface Port of Pi-Hole**

By default, we will set up the Docker container so Pi-Hole will be accessible through port`80` on your system. This could be problematic if you already have something operating on port `80`.

To change this, you will want to find the following line and change the number on the left side of the colon (`:`).

```yaml
      - "80:80/tcp"
```

For example, to change the port to “`8080`“, you would replace that line with the following.

```yaml
      - "8080:80/tcp"
```

#### **Setting the Time Zone for the Pi-Hole Docker Container**

By default, the Pi-Hole docker container has been configured to use the “`Chicago`” time zone. It is possible, however, to adjust this to your local time zone.

To adjust the time zone, find the following line within the file.

```yaml
      TZ: 'America/Montreal'
```

### Saving the Docker-Compose File

**6.** Once you have made the above changes to the file, save and quit by pressing CTRL + X, followed by Y, then the ENTER key.

### Disabling the Systemd-Resolve Service (Ubuntu Only)

**7.** Our next step is to modify the “`/etc/resolv.conf`” file to point to a different nameserver. By default, the nameserver will be configured your homenetwork DNS server.

Use the command below to begin modifying the configuration file.

```bash
sudo nano /etc/resolv.conf
```

**7.** You will want to find and replace the following line within this file.

```
nameserver 127.0.0.53
```

Replace it with the following. This changes the nameserver to Cloudflare’s 1.1.1.1 service.

```
nameserver 1.1.1.1
```

**8.** Once you have made changes to this file, save and quit by pressing CTRL + X, followed by Y, then the ENTER key.

### Starting the Pi-Hole Docker Container

**9.** We can finally start up Pi-Hole’s Docker container on our Linux system.

All you need to do now is run the following command within the terminal.

```bash
sudo docker-compose up -d
```

Please note this process can take a couple of minutes, depending on your device’s internet connection.

**If your Pi is already listening on port 80 the previous command will fail. You can either stop the service or modify the yml file**

```
nano docker-compose.yml
```

then change the line

```yaml
      - "80:80/tcp"
```

to.

```yaml
      - "8080:80/tcp"
```

Save the file exit and issue the command

```bash
sudo docker-compose up -d --force-recreate
```



## Accessing the Pi-Hole Web Interface



Now that we have the Pi-Hole docker container up and running on your system, we can proceed to use its web interface.

This web interface allows you to control all aspects of Pi-Hole on your system, so you won’t have to mess around with configuration files.

**1.** Before we begin, you will need to know the IP address of your device so that you can access the web interface.

The easiest way to get the local IP address is to use the hostname command.

```bash
hostname -I
```

**2.** With your local IP address, you will want to go to the following within your web browser.

Ensure you replace “`IPADDRESS`” with the IP you got in the previous step.

```
http://IPADDRESS/admin
```

If you changed the port away from “`80`“, you need to insert the port like shown below.

```
http://IPADDRESS:PORT/admin
```

**3.** You should now be greeted with the login page for Pi-Hole.

To log in, you must type in the password (**1.**) you set when writing the Docker configuration file earlier.

With your password typed in, click the “`Log In`” button (**2.**)

![Pi-Hole Docker Login Screen](https://cdn.pimylifeup.com/wp-content/uploads/2022/10/Pi-hole-docker-container-01-Login-Screen.jpg)



**4.** You now have access to the Pi-Hole dashboard running from within the Docker container.

![Pi-Hole Dashboard](https://cdn.pimylifeup.com/wp-content/uploads/2022/10/Pi-hole-docker-container-02-Pi-Hole-Dashboard.jpg)

**Make a screenshot piHole1.jpg**

**5.** With access to the dashboard, now is a good time to start changing your device’s DNS to use Pi-Hole.

When setting the DNS servers, you must use the IP belonging to the device you are running Pi-Hole on.

**6.** Open a browser window and navigate to a site know to have ads in it.

**7.** Switch to the pi-hole admin interface and click the Query log menu item

**Make a screenshot piHole2.jpg**

Upload all the screenshots to Github.

When you are finished you may want to reset your laptop to its default DNS servers and to stop the pi-hole container using `docker stop`

### Remove one or more specific images

Use the `docker images` command with the `-a` flag to locate the ID of the images you want to remove. This will show you every image, including intermediate image layers. When you’ve located the images you want to delete, you can pass their ID or tag to `docker rmi`:

**List:**

```bash
docker images -a
```

**Remove:**

```bash
docker rmi Image Image
```

## Purging All Unused or Dangling Images, Containers, Volumes, and Networks

Docker provides a single command that will clean up any resources — images, containers, volumes, and networks — that are *dangling* (not tagged or associated with a container):

```bash
docker system prune
```

To additionally remove any stopped containers and all unused images (not just dangling images), add the `-a` flag to the command:

```bash
docker system prune -a
```