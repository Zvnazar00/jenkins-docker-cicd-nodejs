# Jenkins CI/CD Pipeline for a Node.js App (Vagrant + Docker Compose)

DevOps project: Jenkins master/worker infrastructure provisioned with Vagrant, running a CI/CD pipeline for a Node.js application — build, automated tests and publishing the Docker image to DockerHub.

## What's implemented

- **Infrastructure (Vagrant)** (`Vagrantfile`):
  - `jenkins-master` (`192.168.56.10`) and `jenkins-worker` (`192.168.56.11`) on a private network
  - Provisioning via shell scripts (`provision_master.sh`, `provision_worker.sh`)

- **Application**: a minimal **Express** server (`app/index.js`), containerized with a `Dockerfile`.

- **Tests**: automated tests with **Jest + Supertest** (`tests/app.test.js`), run in a separate Docker container against the running application.

- **Docker Compose** (`docker-compose.yml`): brings up the application and the test runner on the same network, with a health check on the application — tests start only once the service is ready to accept requests.

- **CI/CD pipeline (Jenkinsfile)**:
  1. Checkout the repository
  2. `docker compose build` — build the application image
  3. Run the test container (`docker compose up --exit-code-from`)
  4. On success — publish the image to DockerHub
  5. Separately handle the case of failed tests

## Tech stack

`Vagrant` · `Jenkins` · `Docker` / `Docker Compose` · `Node.js / Express` · `Jest` · `Groovy (Jenkinsfile)`

## Architecture

1. Vagrant brings up two VMs: the Jenkins master and a Jenkins worker (the agent that runs pipelines).
2. The Jenkins pipeline builds the Node.js application's Docker image and runs the tests in an isolated container via `docker compose`.
3. Once the tests pass, the image is published to DockerHub.

## Repository structure

```
Src/
├── vagrant_conf/
│   ├── Vagrantfile
│   ├── provision_master.sh
│   └── provision_worker.sh
└── step_project_2/
    ├── app/                 # Express application
    ├── tests/                # Jest/Supertest tests
    ├── docker-compose.yml
    └── Jenkinsfile
Screens/                      # Screenshots of the deployment and CI/CD run
```

## Screenshots

Screenshots of Jenkins and the pipeline in action are in the `Screens/` folder.
