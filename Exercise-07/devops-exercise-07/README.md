# Exercise 07: Introduction to Continuous Integration (CI) and Jenkins Installation

## Objective
Understand Continuous Integration (CI), review common CI/CD tools, and install Jenkins using Docker.

## What is Continuous Integration (CI)?
Continuous Integration is a development practice in which developers frequently integrate code changes into a shared repository. Automated builds and tests help identify errors early, improve software quality, and speed up delivery.

## Key Features of CI
1. **Frequent code integration:** Developers commit changes to a shared repository regularly.
2. **Automated builds:** Code changes trigger a build to validate the integration.
3. **Automated testing:** Tests run as part of the CI process.
4. **Immediate feedback:** Developers receive quick feedback about build and test results.

## Benefits of CI
- Early bug detection.
- Improved collaboration.
- Faster development cycles.
- Better code quality and fewer regressions.

## How CI Works
1. A developer commits and pushes code to version control.
2. A CI server detects the changes.
3. The server builds the application and resolves dependencies.
4. Automated tests run.
5. The results are reported to the team.

## Examples of CI/CD Tools
- **GitLab CI/CD:** CI/CD integrated with GitLab repositories and YAML-based pipelines.
- **CircleCI:** Cloud CI/CD with container support, caching, and parallelism.
- **Travis CI:** CI service with YAML configuration and GitHub integration.
- **Bamboo:** Atlassian CI/CD tool that integrates with Jira and Bitbucket.
- **TeamCity:** JetBrains CI/CD server with build runners and integrations.
- **Azure DevOps Pipelines:** Microsoft pipeline service with YAML and classic editors.
- **GitHub Actions:** Repository-integrated workflows defined in YAML.
- **Spinnaker:** Open-source continuous delivery platform for cloud-native applications.
- **Buildkite:** Hybrid CI/CD platform with self-hosted agents.
- **Drone:** Container-oriented CI/CD platform.

## Introduction to Jenkins
Jenkins is an open-source automation server used to automate building, testing, and deploying software. Plugins extend Jenkins functionality, and distributed builds can run across multiple machines.

### Key Jenkins Concepts
1. **Job/Project:** A task configured to run in Jenkins.
2. **Build:** A run of a job or pipeline.
3. **Pipeline:** A sequence of steps for building, testing, and deploying.
4. **Plugins:** Extensions that integrate Jenkins with other tools.
5. **Nodes:** Machines that execute Jenkins workloads.

## Install Jenkins with Docker

Run the following command in a terminal with Docker available:

```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
```

- Port `8080` exposes the Jenkins web interface.
- Port `50000` is used for inbound agent connections in setups that use this connection method.
- Docker downloads the image if it is not already available locally.

Check the container:

```bash
docker ps -a
```

Retrieve the initial administrator password:

```bash
docker exec -it jenkins bash
cat /var/jenkins_home/secrets/initialAdminPassword
exit
```

Open Jenkins in a browser:

```text
http://localhost:8080/
```

Enter the initial administrator password when prompted, then follow the setup wizard. The supplied screenshots show Jenkins' plugin installation progress and the Jenkins home page.

## Verification
- The Jenkins container was created and shown as running in Docker.
- The Jenkins web interface was accessible at `http://localhost:8080/`.
- The setup wizard reached plugin installation and the Jenkins home page was displayed.

## Screenshots
Screenshots captured during the exercise are available in the `screenshots/` directory:
- `jenkins-container-running.png`
- `jenkins-plugin-installation.png`
- `jenkins-plugin-setup-progress.png`
- `jenkins-home-page.png`

## Result
Jenkins was started in Docker and its web interface was opened. The installation wizard and Jenkins home page were captured as evidence.
