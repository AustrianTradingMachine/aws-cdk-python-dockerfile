# AWS CDK Python 3.9 Development Environment

This repository contains a Dockerfile for setting up a development environment for AWS Cloud Development Kit (CDK) using Python 3.9 on Debian Bullseye (11). The image includes Node.js, AWS CLI, and other necessary tools to work with AWS CDK.

## Features

- **Base Image**: Debian Bullseye (11) with Python 3.9.
- **Node.js**: Installed via nvm (Node Version Manager) to manage different Node.js versions.
- **AWS CLI v2**: Installed and configured based on system architecture (amd64/arm64).
- **AWS CDK**: Globally installed for infrastructure as code using Python and AWS.
- **Python Packages**: Python development packages such as pip, venv, setuptools, and wheel are pre-installed.
- **Non-root User**: The image runs as a non-root user (`developer`) for better security.
- **Microsoft Dec Container**: Prepared for usage in the Microsoft Dev Container in Visual Studio Code.
  

## Getting Started

### Prerequisites

- Docker installed on your local machine.
- A copy of this repository.

### Building the Docker Image

To build the Docker image, navigate to the directory containing the Dockerfile and run the following command:

```bash
docker build -t aws-cdk-python3.9 .
```

This command will create a Docker image named `aws-cdk-python3.9`.

### Running the Docker Container

Once the image is built, you can start a container using:

```bash
docker run -it --rm -v $(pwd):/workspaces aws-cdk-python3.9
```

This command runs the container in interactive mode (`-it`), removes it after exit (`--rm`), and mounts the current directory into the container's `/workspaces` directory.

### Using the Container

- **Working Directory**: The container's default working directory is `/workspaces`. Any files you work on will be synced with your host machine's directory.
- **Python**: Use `python3` to run Python scripts.
- **AWS CDK**: Use `cdk` commands for AWS CDK operations.
- **AWS CLI**: Use `aws` to interact with AWS services.

### Customizing the Environment

You can modify the `requirements.txt` and `requirements_dev.txt` files to include additional Python packages. The Dockerfile is set up to automatically install these dependencies when building the image.

### Default included packages:
* aws-cdk-lib
* constructs
* boto3
* pytest

## Troubleshooting

If you encounter issues, consider the following steps:

- **Rebuild the Image**: Sometimes changes might not reflect due to caching. Rebuild the image with the `--no-cache` option:
  ```bash
  docker build --no-cache -t aws-cdk-python3.9 .
  ```
- **Check Logs**: Use the `docker logs <container_id>` command to view container logs if it fails to start.

## Contributing

Feel free to open issues or pull requests if you have any suggestions or improvements.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## References:
* [Debian Docker Image](https://hub.docker.com/_/debian)
* [VSCode Container](
https://code.visualstudio.com/docs/devcontainers/containers)
* [Amazon Linux](https://docs.aws.amazon.com/linux/)
