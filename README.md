

---

# C Application Docker CI/CD Pipeline

This repository contains a simple C program that is containerized using Docker and automatically built and pushed to Docker Hub using a GitHub Actions CI/CD workflow.

---

## Features

* Simple C program (main.c)
* Dockerfile that compiles the program using GCC
* GitHub Actions CI pipeline:

  * Build Docker image
  * Tag image
  * Push to Docker Hub
* Automated workflow triggered on every push to the main branch

---

## Project Structure

```
.
├── main.c
├── Dockerfile
├── README.md
└── .github
    └── workflows
        └── docker.yml
```

---

## C Program (main.c)

```c
#include <stdio.h>

int main() {
    printf("Hello from C in Docker!\n");
    return 0;
}
```

---

## Dockerfile

```dockerfile
FROM gcc:latest
WORKDIR /app
COPY main.c .
RUN gcc main.c -o app
CMD ["./app"]
```

---

## GitHub Actions Workflow (docker.yml)

```yaml
name: C App Docker CI

on:
  push:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -t c-docker-app .

      - name: Tag image
        run: docker tag c-docker-app ${{ secrets.DOCKER_USERNAME }}/c-docker-app:latest

      - name: Push to Docker Hub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/c-docker-app:latest
```

---

## GitHub Secrets Required

Go to GitHub Repository → Settings → Secrets → Actions
Create the following secrets:

| Secret Name     | Description                              |
| --------------- | ---------------------------------------- |
| DOCKER_USERNAME | Your Docker Hub username                 |
| DOCKER_PASSWORD | Your Docker Hub password or access token |

---

## CI/CD Pipeline

1. Push code to GitHub
2. GitHub Actions runs automatically
3. Docker image is built
4. The image is tagged
5. The image is pushed to Docker Hub

---

## Test Image from Docker Hub

```
docker pull YOUR_DOCKER_USERNAME/c-app:latest
docker run YOUR_DOCKER_USERNAME/c-app:latest
```

Expected output:

```
Hello from C in Docker!
```

---




