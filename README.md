Dit project is een docker compose bestand die Jenkin en SonarQube LTS versie starten.

# Dashboards

- [Jenkins Dashboard](http://127.0.0.1:7080/jenkins/)
- [SonarQube Dashboard](http://127.0.0.1:9000/sonarqube/)

## Install plugins

De open source versie van Sonar ondersteunt standaard geen multibranch project, omdit mogelijk te maken kunnen we
de [sonarqube-community-branch-plugin](https://github.com/mc1arke/sonarqube-community-branch-plugin) plugin gebruiken.

Download de [sonarqube-community-branch-plugin](https://github.com/mc1arke/sonarqube-community-branch-plugin/releases/download/1.3.2/sonarqube-community-branch-plugin-1.3.2.jar),
plaats de plugin in de folder `data\sonarqube\plugins` en controleer of het versie nummer in de `docker-compose.yaml` hetzelfde is.

## Opstarten en afsluiten

Om te starten voer het command uit (`-d` zorgt dat Jenkins en SonarQube als een achtergrond services gestart worden):

```shell
docker-compose up -d
```

Om af te sluiten voer het commando uit:

```shell
docker-compose down
```

# Jenkins configuratie
Als Jenkins is opgestart en het [Dashboard](http://127.0.0.1:7080/jenkins/) opent,
krijg je de vraag "Customize Jenkins" kies dan "Install suggested plugins".

Als is de installatie is afgerond, ga dan naar de [Update Center](http://127.0.0.1:9080/updateCenter/) en installeer de volgende plugins:
- SonarQube Scanner
- Locale
- OWASP Dependency-Check
- Build Monitor View

## Stel de standaard taal in

1. Login Jenkins
1. Ga naar "Beheer Jenkins->Configureer System->Locale"
1. Voor "Default Language" vul de waarde "en_IE". (Engelse taal met Nederlands datum/tijd)
1. Vink het vakje onder "Default Language" aan ("Ignore browser preference and force this language to all users")


## Stel de bouw agents in
1. Login Jenkins
1. Ga naar "Beheer Jenkins->Configureer System"
1. Vul voor "# of executors" de waarde 1 in (de rest gaat via de nodes)
1. Klik op "Save"
1. Goto "Manage Jenkins->Manage Nodes and Clouds"
1. Click on "New Node"
1. For "Node name" set the value "jenkins-agent-one"
1. Select "Permanent Agent"
1. Click "OK"
1. For "Remote root directory" set the value "/home/jenkins/agent".
1. Click "OK"

## Get node secret
1. Login Jenkins as admin
1. Goto "Manage Jenkins->Manage Nodes and Clouds"
1. Click on "jenkins-agent-one"
1. Copy the value after "-secret"
1. Past the value in the new file "./secrets/jenkins-agent-one.txt"

# Install Global tools

## JDK

Lookup the download link here: https://adoptopenjdk.net/releases.html

1. Click "Manage Jenkins" > "Global Tool Configuration" > "Add JDK" (near JDK installations)
1. Delete the java.sun.com installer. Just click "Add Installer" below and choose "Extract .zip/.tar.gz"
1. Download URL: copy from link above
1. "Save" the configuration
