# Continuous Integration Tool Kit

This docker stack helps you to deploy a Jenkins pipeline with security analysis.
The stack includes:
* Jenkins
* Java 11
* Sonarqube Community Edition (Static analysis)
* Owasp Dependency Track (Software Composition analysis)
* OWASP ZAP (Pentesting tool)
* Aquasec/trivy (docker image analysis)
* Docker bench security (Docker container and docker daemon analysis)
* ClamAV (Malware analysis tool)

## Deploy tools
Just run:
```bash
sudo sysctl -w vm.max_map_count=262144 ## needed to deploy sonarqube
docker build -t my_aquasectrivy -f ./aquasectrivy/Dockerfile .
docker-compose up -d
```

## Continuous Integration tool
Jenkins is running at:
```bash
htttp://localhost:8080
```
You can create your projects and pipelines. All the information is persisted as there is a specific volume for that.

## SAST analysis
SAST analysis are run with Sonarqube community edition.
Sonarqube is running at:
```bash
htttp://localhost:9000
```
You can create your projects and run analysis, all the information is persisted in a postgresql database.

## SCA analysis
SCA analysis are run with Owasp Dependency Track.
Owasp Dependency Track is running at:
```bash
http://localhost:8180
```
You can create your projects and run analysis, all the information is persisted.

## Pentesting analysis
Pentesting analysis are run with OWASP ZAP tool.
You can download/upload owasp sessions.
OWASP ZAP is running at:
```bash
htttp://localhost:8081/zap
```

## Docker image analysis
To analyze docker image analysis the stack includes the tool aquasec/trivy.
The steps to run the tool are:
```bash
cd aquasectrivy
docker build -t my_aquasectrivy .
docker run -it my_aquasectrivy bash
```
It opens a command shell and we can execute the command:
```bash
trivy {MY_DOCKER_IMAGE}
example: trivy sebastianrevuelta/chess-game:latest
```
If you want to analyze a docker image that is not pushed in the repository (it exists only at local level) then run the command:
```bash
docker run -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/Library/Caches:/root/.cache/ -it my_aquasectrivy bash
```
and then you can run the *trivy* command as explained before.

## Docker container and docker daemon analysis
To analyze docker containers in production and docker daemon configuration the stack includes the tool docker security bench. 
The tests are all automated, and are inspired by the CIS Docker Benchmark v1.2.0.
To execute a check of all your containers and docker daemon configuration you can run the next command:
```bash
cd dockerbench
./check.sh
```

## Malware analysis
Malware analysis are run with clamAV engine. 
clamAV is inside remnux distro (docker image).

### Usage
To execute a malware analysis you need to execute:
```bash
docker exec -it citoolkit_remnux_1 /bin/sh
freshclam
clamscan .
```

