# docker-node-dojo

A Dojo docker image with tools to build and test node.js projects by deductions team.

Tested and released images are published to dockerhub as [nhsdev/node-dojo](https://hub.docker.com/r/nhsdev/node-dojo)

## Usage
1. Setup docker.
2. Install [Dojo](https://github.com/kudulab/dojo) binary.
3. Provide a Dojofile at the root of the project:
```
DOJO_DOCKER_IMAGE="nhsdev/node-dojo:<commit>"
```
4. Create and enter the container by running `dojo` at the root of project.

By default, current directory in docker container is `/dojo/work`.


## Flavours

| Image                            | Base Image       | Notes                                  |                                 
|:---------------------------------|------------------|:---------------------------------------|
| `nhsdev/node-dojo:<commit>`      | `node:12-alpine` |                                        |
| `nhsdev/node-dojo:<commit>-slim` | `node:12-slim`   | Includes dependencies to run pa11y-ci. |
