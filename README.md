# CI/CD with GitHub Actions and Docker

## Introduction

This project demonstrates how **Continuous Integration (CI)** and part of **Continuous Delivery (CD)** can be implemented using **GitHub Actions**, **Docker**, and **Docker Hub**.

The application consists of:

* A React frontend
* A Node.js/Express backend
* Dockerfiles for each application
* Docker Compose for running the application locally
* GitHub Actions workflows for automatically building Docker images

The goal is to automate repetitive tasks so that every change made to the project is tested and built consistently.

---

# What is CI/CD?

CI/CD stands for:

* **Continuous Integration (CI)**
* **Continuous Delivery (CD)** or **Continuous Deployment (CD)**

Although people often say "CI/CD" as one term, they represent two related but different practices.

![](</screenshots/stage5img/CI,CD - relay race - made with Gemini.png>)

---

## Continuous Integration (CI)

Continuous Integration is the practice of automatically checking changes made to a project whenever code is pushed to a shared repository.

Instead of waiting until the end of development to verify whether everything still works, CI checks the project immediately after new code is committed.

A CI pipeline may perform tasks such as:

* Downloading the project's source code
* Installing dependencies
* Checking code formatting
* Running linting
* Running automated tests
* Building the application
* Building Docker images

If any of these steps fail, the pipeline stops and reports the error.

This allows developers to detect problems early rather than discovering them much later.

**frontend ci**
![](</screenshots/stage5img/frontend ci,cd passed.png>)


**backend ci**
![](</screenshots/stage5img/backend ci,cd.png>)


---

## Continuous Delivery (CD)

Continuous Delivery extends CI.

Once the application has successfully passed all CI checks, CD prepares it so it is ready for deployment.

Examples include:

* Building production Docker images
* Uploading images to Docker Hub
* Packaging release artifacts
* Preparing deployment packages

The application is ready to be deployed whenever someone decides to deploy it.

**frontend docker image**
![](</screenshots/stage5img/frontend-image.png>)


**backend docker image**
![](</screenshots/stage5img/backend-image.png>)


---

## Continuous Deployment
*the other CD*

Continuous Deployment goes one step further.

Instead of stopping after preparing the application, it automatically deploys the latest successful version to production without requiring manual approval.

For example:

1. Developer pushes code.
2. CI verifies everything.
3. Docker image is built.
4. Image is uploaded.
5. Production servers automatically update.

Not every project uses Continuous Deployment.

Many organisations use Continuous Delivery instead because they want a person to approve deployments.

---

# Why is CI/CD Important?

Without CI/CD, developers perform many tasks manually.

For every code change they might need to:

* Install dependencies
* Run tests
* Build the application
* Build Docker images
* Push images to Docker Hub
* Deploy the application

Doing these manually introduces inconsistency because people may forget steps or perform them differently.

CI/CD automates these repetitive tasks so they happen the same way every time.

Some benefits include:

* Faster feedback
* Earlier detection of errors
* Consistent build process
* Reduced manual work
* Easier collaboration between developers
* More reliable software releases

Automation also makes projects easier to maintain as they grow.

---

# What is GitHub Actions?

GitHub Actions is GitHub's automation platform.

It executes predefined workflows whenever specific events occur inside a repository.

Examples of events include:

* Pushing code
* Creating a pull request
* Publishing a release
* Creating a tag

A workflow is written in YAML and stored inside:

```text
.github/workflows/
```

![](</screenshots/stage5img/GitHub Actions workflow.png>)


Each workflow contains one or more jobs.

Each job contains one or more steps.

For example, a workflow may:

1. Check out the repository.
2. Install Node.js.
3. Install project dependencies.
4. Build the project.
5. Build a Docker image.
6. Log in to Docker Hub.
7. Push the image to Docker Hub.

Everything happens automatically after the workflow is triggered.

---

# How CI/CD Works in This Project

The workflow for this project follows these general steps.

```text
Developer
    │
    │ git push
    ▼
GitHub Repository
    │
    ▼
GitHub Actions Workflow Starts
    │
    ├── Checkout source code
    ├── Set up Node.js (if required)
    ├── Build application
    ├── Build Docker image
    ├── Log in to Docker Hub
    └── Push Docker image
    │
    ▼
Docker Hub
```

This means every successful push can automatically produce an updated Docker image without manually running Docker commands.

---

# What is a Dockerfile?

A Dockerfile is a text file containing instructions for building a Docker image.

Think of it as a recipe that tells Docker exactly how to package an application.

A Dockerfile may include instructions such as:

* Which base image to use
* Which files to copy
* Which dependencies to install
* Which ports to expose
* Which command starts the application

For example, a Node.js Dockerfile typically:

1. Starts from a Node.js base image.
2. Sets a working directory.
3. Copies package files.
4. Installs dependencies.
5. Copies the application source code.
6. Exposes the application's port.
7. Starts the server.

Every time Docker builds the image, it follows these instructions in order.

Because the instructions are written down, every developer and every CI pipeline produces the same image.

![](</screenshots/backend.png>)

---

# Why Does This Project Have Separate Dockerfiles?

This project contains two different applications:

* Frontend
* Backend

Each application has different requirements.

The frontend needs to build React into static files before serving them.

The backend runs a Node.js server.

Since they perform different tasks, each application has its own Dockerfile.

This allows each image to be built independently.

---

# What is Docker Compose?

Docker Compose is a tool for running multiple containers together using a single configuration file.

Instead of starting containers individually, Docker Compose starts every required service with one command.

A typical application often needs multiple containers.

For example:

* Frontend
* Backend
* PostgreSQL database

These services depend on one another.

Docker Compose defines:

* Which containers exist
* Which Dockerfile or image each service uses
* Environment variables
* Networks
* Volumes
* Port mappings
* Startup configuration

Everything is described inside a `docker-compose.yml` (or `compose.yaml`) file.

Running:

```bash
docker compose up
```

starts the complete application stack.

![](</screenshots/docker compose up.png>)

---

# How Docker Compose Relates to This Project

In this project, Docker Compose is used for local development.

It starts:

* the frontend container,
* the backend container, and
* the PostgreSQL database container

at the same time.

It also:

* connects them to the same Docker network,
* passes required environment variables,
* maps ports to the host machine, and
* creates a persistent Docker volume for PostgreSQL data.

Because all services are defined in one file, anyone who clones the repository can start the entire application using a single command.


<!--- this is a multi-line comment

---

# Relationship Between Dockerfiles and Docker Compose

Although they work together, they have different responsibilities.

| Dockerfile                                  | Docker Compose                             |
| ------------------------------------------- | ------------------------------------------ |
| Builds one Docker image.                    | Runs one or more containers.               |
| Describes how an image is created.          | Describes how containers work together.    |
| Used during image creation.                 | Used when starting services.               |
| One application usually has one Dockerfile. | One Compose file can manage many services. |

In this project:

* each Dockerfile defines **how** to build either the frontend or backend image;
* Docker Compose defines **how those images are run together** with the PostgreSQL database.

multi-line comment ends here --->

---

# How GitHub Actions Uses the Dockerfiles

GitHub Actions does not use Docker Compose to build the production images in this project.

Instead, each workflow directly builds the required Docker image using its corresponding Dockerfile.

For example, the frontend workflow builds the frontend image from the frontend Dockerfile before pushing it to Docker Hub.

Docker Compose remains useful for running the complete application locally, while GitHub Actions focuses on automating the image build process.

---

# Summary

This project combines several technologies that each solve a different problem.

* **GitHub Actions** automates the build pipeline.
* **Continuous Integration (CI)** automatically verifies changes whenever code is pushed.
* **Continuous Delivery (CD)** prepares successful builds for deployment by creating and publishing Docker images.
* **Dockerfiles** define how individual application images are built.
* **Docker Compose** starts multiple containers together for local development and testing.
* **Docker Hub** stores the built Docker images so they can be downloaded and run elsewhere.

Together, these tools create a repeatable and reliable workflow that reduces manual effort, improves consistency, and makes it easier to build and distribute the application.
