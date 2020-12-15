# Continuous Integration Tool Kit

This docker stack helps you to deploy a Jenkins pipeline with security analysis.
The stack includes:
* Jenkins
* Java 11
* Sonarqube Developer Edition (SAST tool, license needed)
* OWASP ZAP (Pentesting tool)
* Aquasec/trivy (docker image analysis)
* ClamAV (Malware analysis tool)

## Continuous Integration tool
Jenkins is running at:
```bash
htttp://localhost:8080
```
You can create your projects and pipelines. All the information is persisted as there is a specific volume for that.

## SAST analysis
SAST analysis are run with Sonarqube developer edition (so you will need a license for that).
Sonarqube is running at:
```bash
htttp://localhost:9000
```
You can create your projects and run analysis, all the information is persisted in a postgresql database.

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

