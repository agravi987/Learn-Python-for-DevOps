# 07-Docker-Automation

## docker Library

- Install: `pip install docker`
- Import: `import docker`
- Client: `client = docker.from_env()`

## Operations

- Create container: `container = client.containers.run("image", detach=True)`
- Stop: `container.stop()`
- Start: `container.start()`
- Build image: `client.images.build(path=".", tag="myimage")`

## Practice

- Automate container lifecycle: Run, check status, logs
- Build and push images via API
