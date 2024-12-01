
## Scanning Images with 'Docker Scout'

```Docker Scout``` is an open source tool from Docker which provides security scanning capabilities for various artifacts including container images, OS packages and application dependencies

Docker Scout is a standalone service and platform that you can interact with using Docker Desktop, Docker Hub, the Docker CLI, and the Docker Scout Dashboard. Docker Scout also facilitates integrations with third-party systems, such as container registries and CI platforms.

The home page for Docker Scout is at [https://docs.docker.com/scout/](https://docs.docker.com/scout/)


### Installing Docker Scout

The Docker Scout CLI has a Github repo at [https://github.com/docker/scout-cli](https://github.com/docker/scout-cli)

Docker Scout is a single Go static binary.

### Download the 1.15.1 Release tar archive

Perform the following steps to install and validate Docker Scout:

```

mkdir -p ~/tmp/docker-scout
cd       ~/tmp/docker-scout

wget https://github.com/docker/scout-cli/releases/download/v1.15.1/docker-scout_1.15.1_linux_amd64.tar.gz

tar xf docker-scout_1.15.1_linux_amd64.tar.gz  docker-scout
sudo mv docker-scout /usr/local/bin/

```

Validate that the 1.15.1 version was correctly installed:
```
docker-scout version
```

Then remove the unneeded tar file:
```
rm docker-scout_1.15.1_linux_amd64.tar.gz

cd
```

### Running Docker Scout

#### Login to Docker Hub

Note that you will be prompted to login to the Docker Hub before using Docker Scout.

You can either
- use the ```docker login``` command to login prior to launching Docker Scout
- Use the login link provided by Docker Scout to allow login in your browser

Run Docker Scout on the latest nginx image as follows:
```
docker-scout cves nginx:latest
```

This provides detailed information about vulnerabilities in the image with a fina summary of the form
```
52 vulnerabilities found in 24 packages
  CRITICAL  0
  HIGH      0
  MEDIUM    0
  LOW       52
```

Look also at the detail list of vulnerabilities - Note that as per other tools, detailed information is provided about each known vulnerability along with it's severity and a link to an information page about the vulnerability (CVE - "Common Vulnerability and Exposures")

The tool terminates with
```
What's next:
    View base image update recommendations → docker scout recommendations nginx:latest
```

Run the ```recommendations``` sub-command on the image to see what base image updates it proposes if any.


### Explore an older nginx image

First view again just the summary line for this latest image using:

```docker-scout cves nginx:latest |& grep Total:```

Then compare this against an older nginx image, e.g.

```docker-scout cves nginx:1.25 |& grep Total:```

**Note:** You can see what image tags (versions) exist by searching for nginx on the Docker Hub - the following link shows those tags <a href="https://hub.docker.com/_/nginx" > https://hub.docker.com/_/nginx </a> for the official nginx image

### Extra: Investigate other images

Investigate some other images of interest
- consider images used, or built in earlier labs
- search for images of interest in the Docker Hub at [https://hub.docker.com](https://hub.docker.com)

