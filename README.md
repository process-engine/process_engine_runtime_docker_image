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

Die ProcessEngine API ist unter `http://localhost:8000` erreichbar.

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


### Wen kann ich auf das Projekt ansprechen?
* [Robin Lenz](mailto:robin.lenz@5minds.de)
