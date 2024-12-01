
## Scanning Images with Trivy

```Trivy``` is  an open source tool from AquaSecurity which provides security scanning capabilities for various artifacts including container images, OS packages and application dependencies

The home page is at [https://trivy.dev/]( https://trivy.dev/)

### Installing Trivy

The Trivy Github Repository is at [https://github.com/aquasecurity/trivy](https://github.com/aquasecurity/trivy)

Trivy is a single Go static binary which can be downloaded from the releases page at [https://github.com/aquasecurity/trivy/releases/](https://github.com/aquasecurity/trivy/releases/)

### Download the 0.57.1 Release tar archive

Perform the following steps to install and validate Trivy:

```

mkdir -p ~/tmp/trivy
cd       ~/tmp/trivy

wget https://github.com/aquasecurity/trivy/releases/download/v0.57.1/trivy_0.57.1_Linux-64bit.tar.gz
tar xf trivy_0.57.1_Linux-64bit.tar.gz
sudo rsync -av trivy contrib /usr/local/bin/
```

Validate that the 0.57.1 version was correctly installed:
```
trivy --version
```

Then remove the unneeded tar file:
```
rm trivy_0.57.1_Linux-64bit.tar.gz

cd
```

### Running Trivy

Run Trivy on the latest nginx image as follows:
```
trivy image nginx:latest
```

This command will provide detailed information about known vulnerabilities found in the container image along with a summary line:
```
Total: 141 (UNKNOWN: 0, LOW: 93, MEDIUM: 30, HIGH: 16, CRITICAL: 2)
```

Look at the detailed list of vulnerabilities, note their severity.

Follow the link provided for one of the vulnerabilities to see the documentation about it.


### Explore an older nginx image

First view again just the summary line for this latest image using:

```trivy image nginx:latest |& grep Total:```

Then compare this against an older nginx image, e.g.

```trivy image nginx:1.25 |& grep Total:```

**Note:** You can see what image tags (versions) exist by searching for nginx on the Docker Hub - the following link shows those tags [https://hub.docker.com/_/nginx](https://hub.docker.com/_/nginx) for the official nginx image

Trivy is a very useful security tool - it's static scanning feature provides immensely useful information about the known vulnerabilities and their severity.

### Extra: Investigate other images

Investigate some other images of interest
- consider images used, or built in earlier labs
- search for images of interest in the Docker Hub at [https://hub.docker.com](https://hub.docker.com)

