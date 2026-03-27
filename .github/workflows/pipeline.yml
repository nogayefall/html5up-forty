name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:

  build:
    name: Build Docker Image
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/html5up-forty:latest

  scan-trivy:
    name: Security Scan with Trivy
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Scan Docker image with Trivy
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ secrets.DOCKER_USERNAME }}/html5up-forty:latest
          format: table
          exit-code: '0'
          severity: CRITICAL,HIGH

  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    needs: scan-trivy
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Deploy
        run: echo "Déploiement effectué avec succès !"