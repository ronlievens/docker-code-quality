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


## Stel standaard bouw node in
1. Login Jenkins
1. Ga naar "Manage Jenkins->Manage Nodes and Clouds"
1. Klik op "Built-In Node" en daarna klik op "Configure"
1. Voor "Number of executors" vul de waarde "0" in
1. Voor "Usage" vul de waarde "Only build jobs with label expressions matching this node" in
1. Klik op "Save"


## Stel de eerste bouw agent in
1. Login Jenkins
1. Ga naar "Manage Jenkins->Manage Nodes and Clouds"
1. Klik op "New Node"
1. Voor "Node name" vul de waarde "jenkins-agent-one" in
1. Selecteer "Permanent Agent"
1. Klik op "Create"
1. Voor "Number of executors" vul de waarde "3" in
1. Voor "Remote root directory" vul de waarde "/home/jenkins/agent" in
1. Click "Save"


## Stel de tweede bouw agent in
1. Login Jenkins
1. Ga naar "Manage Jenkins->Manage Nodes and Clouds"
1. Klik op "New Node"
1. Voor "Node name" vul de waarde "jenkins-agent-two" in
1. Selecteer "Permanent Agent"
1. Klik op "Create"
1. Voor "Number of executors" vul de waarde "3" in
1. Voor "Remote root directory" vul de waarde "/home/jenkins/agent" in
1. Click "Save"


## Stel node secret voor de eerste bouw agent in
1. Login Jenkins
1. Ga naar "Beheer Jenkins->Manage Nodes and Clouds"
1. Klik op "jenkins-agent-one"
1. Kopieer de waarde achter "-secret"
1. Plak deze waarde in het bestand "data/jenkins/agents/secrets/jenkins-agent-one.txt"  (Het kan zijn dat docker een folder met dezelfde naam aanmaakt, verwijder die en creeer een nieuw text bestand)


## Stel node secret voor de tweede bouw agent in
1. Login Jenkins
1. Ga naar "Beheer Jenkins->Manage Nodes and Clouds"
1. Klik op "jenkins-agent-one"
1. Kopieer de waarde achter "-secret"
1. Plak deze waarde in het bestand "data/jenkins/agents/secrets/jenkins-agent-two.txt"  (Het kan zijn dat docker een folder met dezelfde naam aanmaakt, verwijder die en creeer een nieuw text bestand)
