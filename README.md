# Supported tags and respective Dockerfile links

* `latest`, `master` [(master/Dockerfile)](https://github.com/process-engine/process_engine_runtime/blob/master/Dockerfile)
* `develop` [(develop/Dockerfile)](https://github.com/process-engine/process_engine_runtime/blob/develop/Dockerfile)

# ProcessEngine Runtime Docker Fullimage

Dieses Projekt beinhaltet eine Dockerfile um eine vollfunktionierende ProcessEngine Runtime.

Es werden die NPM packages der process-engine-runtime verwendet
und alle Abhängigkeiten installiert

## Was sind die Ziele dieses Projekts?

Ziel dieses Projektes ist es, ein vollumfängliches Docker image zur Benutzung
der ProcessEngine Runtime zu erzeugen.

## Relevante URLs

### Website

Weitere Informationen befinden sich auf der process-engine Website:
https://www.process-engine.io

### Docker Hub

https://hub.docker.com/r/5minds/process_engine_runtime/

### GitHub

https://github.com/process-engine/process_engine_runtime_docker_image

## Wie kann ich das Projekt aufsetzen?

Der Container lässt sich mit folgendem Befehl starten:

```shell
docker run -p 8000:8000  5minds/process_engine_runtime:latest
```

Nach dem Start, ist die ProcessEngine API ist unter
`http://localhost:8000` erreichbar.

Es kann bei Start der Docker-Containers ein eindere Port gewählt werden.

### Ablageort von Daten und Konfiguration

Standardmäßig werden alle Daten und die Konfiguration in zwei Volumes abgelegt:

1. process-engine-db
1. process-engine-configuration

Beide Volumes werden automatisch mit dem Start des Containers erstellt und gemountet.

**Benutzerdefinierte Volumes benutzen**

1. Volumes anlegen

   ```bash
   docker volume create process-engine-storage
   docker volume create process-engine-configuration
   ```

2. Container mit Volume starten

   ```bash
   docker run \
     --mount source=process-engine-storage,target=/root/.config/process_engine_runtime/ \
     --mount source=process-engine-configuration,target=/usr/local/lib/node_modules/@process-engine/process_engine_runtime/config/ \
     \
     --publish 8000:8000 \
     \
     5minds/process_engine_runtime:latest
   ```

### Voraussetzungen

* Docker `>= 17.05`

### Build

Das Image lässt sich wie folgt bauen:

```shell
docker build -t 5minds/process_engine_runtime:latest .
```

Es ist möglich, das base image, sowie die Paketversionen anzupassen:

* `NODE_IMAGE_VERSION`: Base image version mit NodeJS und Alpine Linux
* `PROCESS_ENGINE_VERSION`: NPM Version der ProcessEngine

```shell
docker build --build-arg NODE_IMAGE_VERSION=10-alpine \
             --build-arg PROCESS_ENGINE_VERSION=latest \
             --tag 5minds/process_engine_runtime:latest .
```
