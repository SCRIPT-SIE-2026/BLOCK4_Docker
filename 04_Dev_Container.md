# 04. Dev Containers: Developing Inside a Reproducible Environment

## Official documentation

- [Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers), official documentation for the VSCode Dev Containers extension.
- [Advanced Dev Containers](https://code.visualstudio.com/remote/advancedcontainers/overview), cover advanced container configuration when working with the VSCode Dev Containers extension.
- [Dev Container metadata reference](https://containers.dev/implementors/json_reference/), list of all possible metadata fields and their types and examples.

## Why Dev Containers?

So far, we have used **Docker** to:

* run computations,
* persist results using volumes,
* and coordinate multiple containers with **Docker Compose**.

At this point, our scientific workflow is already much more reproducible than a traditional local installation.
This is already useful:
we can run the same code, with the same dependencies, and obtain the same results on different machines.

**However, in research, we often spend much more time developing, testing, debugging, and modifying the code than simply executing it.**

Ideally, we would also like the development environment itself to be:
- reproducible,
- shareable,
- and easy to reuse.

This would make it easier to:
- continue a project several months later,
- onboard students or collaborators,
- or share a research project without requiring everyone to manually recreate the same setup.

A **Dev Container** is an approach where we not only run the project inside a container, but also develop inside it.

With VS Code, the editor still runs on the host machine, but:

* the terminal runs inside the container,
* the commands and tools come from the container,
* the dependencies come from the container,
* and even the VS Code extensions can be defined by the project.

The repository can therefore describe:

* how to execute the workflow,
* but also how to work on it.

The development environment becomes part of the project itself.

The questions now become:

* **How can a project describe its own development environment?**
* **How can VS Code open a repository directly inside a container?**
* **How can we reuse the Docker and Docker Compose configuration we already wrote?**


## Dev Containers

A Dev Container is a development environment described by the project itself.

With the VS Code Dev Containers extension, VS Code can open the repository directly inside a Docker container.

In practice:
- the editor still runs on the host machine,
- but the terminal, tools, dependencies, and project-specific extensions come from the container.

This allows the repository to describe not only how to run the workflow, but also how to develop it.
## The `.devcontainer` Folder

A Dev Container is configured using a folder named `.devcontainer` at the root of the project.
Inside this folder, the main configuration file is named `devcontainer.json`.

The `devcontainer.json` file tells VS Code how to create and open the development container.

### Minimal example of `devcontainer.json` file: 
```json
{
  "name": "My Developement Container",
  "image": "python:3.11"
}
```
This simple example allows you to deploy and open your project inside a container based on image `python:3.11`. 
Here we you use an online, you can obviously pass a custom image of your own.

You can launch `Dev Container` by typing in VScode:

- Open the Command Palette (F1)
- Type "Dev Containers: Reopen in Container"


<details>

### Using an Existing Dockerfile or compose.yml

If the project already contains a `Dockerfile`, the Dev Container can reuse it directly.

- Dockerfile:

```json
{
  "name": "My Developement Container",
  "build": {
    "dockerfile": "../Dockerfile",
    "context": ".."
  },
}
```
- Docker compose:

```json
{
  "dockerComposeFile": [
    "../compose.yml",
    "compose.extend.yml".      // Overriding default compose.yml if need
  ],
  "service": "<service_name>", // Tells VScode which service to connect
    "runServices": [.          // Tells VScode which service need 
    "<service_name>"
  ],
  "workspaceFolder": "/default/workspace/path/in/container/to/open",
  "shutdownAction": "stopCompose"
}
```

</details>

After opening the project in the Dev Container, a VS Code terminal should start directly inside `/app`.
Commands such as:

```bash
python src/compute.py
```
will be executed directly inside `/app`.



## Exercise

1. Create a `.devcontainer` folder at the root of the project repository.
2. Create a `.devcontainer/devcontainer.json` file.
3. Configure it to reuse either the existing `Docker Image`.
1. Open the project with `Dev Containers: Reopen in Container`.
2. In the VS Code terminal, run:

```bash
python src/compute.py
```

6. Check that the results are created from inside the container.
7. Close and reopen the project to verify that the development environment can be recreated.
8. Try to add some customizations to your Dev Container such as VSCode extensions, you can find some hints [here](https://code.visualstudio.com/docs/devcontainers/create-dev-container#_create-a-devcontainerjson-file).
9. Configure your Dev Container to use your existing `compose.yml` and access to `compute` service.
See [Using an Existing Dockerfile or compose.yml](#using-an-existing-dockerfile-or-compose-yml) to help you.
10. Add a `postCreateCommand` in order to install [pre-commit](https://pre-commit.com/)
11. Add a `postStartCommand` in order to run the generation of your python figure.
12. Configure your Dev Container to connect to `report` service of your `compose.yml` 


## Conclusion

A Dev Container makes the development environment part of the project.

Docker allows us to execute a workflow reproducible.
Docker Compose allows us to coordinate several containers.
Dev Containers allow us to develop inside the same kind of reproducible environment.

For research projects, this is a practical way to make code easier to share, easier to restart, and easier to maintain over time.
