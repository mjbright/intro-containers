
## Scanning Images with 'Docker Bench'

```Docker Bench for Security``` is  an open source tool - a script - from Docker which provides security scanning of both container images and also the hosts on which containers will run - It checks for dozens of common best-practices around deploying Docker containers in production. 

The Docker-Bench Github Repository is at [https://github.com/docker/docker-bench-security](https://github.com/docker/docker-bench-security)

### Installing Docker Bench

Install Docker Bench using the below command - to clone the github repo:
```
git clone https://github.com/docker/docker-bench-security.git
```

### Running Docker Bench

Run Docker Bench on the latest nginx image as follows:
```
cd docker-bench-security
sudo sh docker-bench-security.sh
```

The script performs security checks according to the following categories:
```
1 - Host Configuration
2 - Docker daemon configuration
3 - Docker daemon configuration files
4 - Container Images and Build File
5 - Container Runtime
6 - Docker Security Operations
7 - Docker Swarm Configuration
```

Read about options to the script at:

[https://github.com/docker/docker-bench-security?tab=readme-ov-file#docker-bench-for-security-options]( https://github.com/docker/docker-bench-security?tab=readme-ov-file#docker-bench-for-security-options)


