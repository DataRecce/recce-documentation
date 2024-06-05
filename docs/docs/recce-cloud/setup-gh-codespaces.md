---
title: Recce Cloud - Setup GitHub Codespaces
template: embed.html
---

# Recce Cloud - Setup GitHub Codespaces

Steps to setup Recce Cloud with GitHub Codespaces

## 1. Add the following file content to your `.devcontainer/devcontainer.json` file:

```json
{
    "name": "Recce Cloud",
    "image": "infuseai/dev-container-base-image:latest",
    "containerEnv": {
        "RECCE_CI_WORKFLOW_NAME": ".github/workflows/recce_ci.yml", // Path to the Recce CI workflow file
    },
    "customizations": {
        "vscode": {
            "settings": {
                "terminal.integrated.shell.linux": "/bin/bash"
            }
        }
    },
    "forwardPorts": [ 8000 ],
    "portsAttributes": {
        "8000": {
            "label": "Recce Server",
            "onAutoForward": "openBrowser"
        }
    },
    "postStartCommand": "post_start_command"
  }
```
The python package file `requirements.txt` will be processed by default. If your project uses a different method to manage dependencies, you can choose one of the following methods to customize the installation process.

1. Set the `DEPENDENCIES_FILE` environment variable to the path of the python dependencies file.

2. Set the `DEPENDENCIES_SHELL_COMMAND` environment variable to the command to run to install the dependencies.

3. Set the `DEPENDENCIES_SHELL_SCRIPT_PATH` environment variable to the path to the shell script to install the dependencies.

### Optional Environment Variables for `devcontainer.json`:

The following environment variables can be set in the `containerEnv` section of the `devcontainer.json` file:

- `DEPENDENCIES_FILE`
Path to the python dependencies file (Default: `requirements.txt`)

- `DEPENDENCIES_SHELL_COMMAND`
Command to run to install the dependencies

- `DEPENDENCIES_SHELL_SCRIPT_PATH`
Path to shell script to install the dependencies

- `PRE_INSTALL_DEPENDENCIES_COMMAND`
Command to run before installing dependencies

- `POST_INSTALL_DEPENDENCIES_COMMAND`
Command to run after starting the recce server

- `PRE_LAUNCH_RECCE_SERVER_COMMAND`
Command to run before launching the recce server

- `POST_LAUNCH_RECCE_SERVER_COMMAND`
Command to run after launching the recce server

2. Commit the `.devcontainer` folder to your repository.
```bash
git add .devcontainer
git commit -a -s -m "Add devcontainer configuration"
```