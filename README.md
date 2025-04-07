# tmake

Tannin CI and Make pipelines using [Taskfile](https://taskfile.dev/).

## Overview

This project provides standardized build, test, and deployment pipelines for Tannin projects. It replaces the previous Dagger-based implementation with a more lightweight approach using Taskfile, while maintaining the same functionality.

## Installation

1. Install [Task](https://taskfile.dev/installation/):

```sh
# On macOS
brew install go-task

# On Linux
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d -b ~/.local/bin

# On Windows with Chocolatey
choco install go-task
```

2. Install required dependencies for your language:
   - For Go projects: `go`, `golangci-lint`
   - For Python projects: `python`, `pip`
   - For JavaScript projects: `node`, `npm`
   - For Docker projects: `docker`

## Usage

The Taskfile system uses the same `.tmake.json` configuration file format as the previous Dagger-based system. If you don't have this file, you can create it with:

```sh
task init
```

### Basic Commands

```sh
# Run the default pipeline (usually lint, test, build)
task

# Run a specific task
task build
task test
task lint
task push

# Run with a custom config file
task CONFIG_FILE=custom-config.json

# Run in a specific directory
cd ./services/api && task
```

### Available Tasks

- `build`: Build the current project
- `test`: Run tests
- `lint`: Run linting tools
- `push`: Push built artifacts to registry
- `proto`: Run Protocol Buffer compilation
- `migrate`: Run database migrations
- `init`: Initialize a new project
- `dev`: Set up development environment
- `clean`: Clean up build artifacts
- `pr`: Create or update a pull request
- `seed`: Seed databases or test data

### Configuration

Configure your project using the `.tmake.json` file. Here's an example configuration:

```json
{
  "language": "go",
  "tasks": [
    "test",
    "lint",
    "build"
  ],
  "dockerfile": "Dockerfile",
  "output_dir": "build",
  "proto_dir": "proto",
  "platforms": [
    "amd64",
    "arm64"
  ],
  "env": {
    "ENV_VAR1": "value1",
    "ENV_VAR2": "value2"
  },
  "services": {
    "postgres": {
      "image": "postgres:14",
      "port": 5432,
      "env": {
        "POSTGRES_PASSWORD": "password",
        "POSTGRES_USER": "user",
        "POSTGRES_DB": "db"
      }
    }
  },
  "build": {
    "env": {
      "CGO_ENABLED": "0"
    },
    "tags": ["netgo"],
    "build_args": {
      "VERSION": "1.0.0"
    },
    "packages": ["ca-certificates"],
    "gcflags": ["-N", "-l"],
    "binaries": ["api", "worker"]
  }
}
```

## GitHub Actions Integration

This project includes GitHub Actions workflow files that provide CI/CD capabilities. The workflow will automatically:

1. Set up the appropriate language environment
2. Install Task
3. Configure authentication
4. Run the default pipeline

To use it, just commit your code to the repository. The workflows are triggered on push to the main branch and on pull requests.

## Differences from Dagger-based tmake

While the functionality is mostly the same, there are a few differences:

1. **Local Execution**: Tasks run directly on your machine rather than in containers
2. **Simplified Authentication**: Authentication is handled more directly
3. **No Complex Container Orchestration**: We use Docker directly instead of Dagger
4. **Faster Execution**: Tasks generally run faster due to less container overhead
5. **More Customizable**: Each task is easier to customize directly in the Taskfile

## Private Repository Access

To access private repositories:

1. For GitHub, set up your SSH keys or personal access token
2. For buf.build, ensure you have a properly configured .netrc file

## License

Same as the original tmake project.