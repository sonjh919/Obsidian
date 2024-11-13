```
name: openur-backend

on:
  push:
    branches:
    - main

permissions:
    contents: read

env:
    DOCKER_IMAGE_NAME: ${{ secrets.DOCKER_USERNAME }}/openur

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout source code
      uses: actions/checkout@v3

    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
          distribution: 'corretto'
          java-version: '17'

    - name: Set up application.yml
      run: |
        cd ./src/main/resources
        touch ./application.yml
        echo -e "${{ secrets.APPLICATION }}" > ./application.yml

    - name: Grant execute permission for gradlew
      run: chmod +x ./gradlew
      shell: bash

    - name: Build with Gradle
      run: ./gradlew clean build
      shell: bash

    - name: Build the Docker image
      run: docker build . --file Dockerfile --tag ${{ env.DOCKER_IMAGE_NAME }}:latest

    - name: Login to Docker Hub using Access Token
      run: echo ${{ secrets.DOCKER_HUB_TOKEN }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin

    - name: Push the Docker image
      run: docker push ${{ env.DOCKER_IMAGE_NAME }}:latest

    - name: Deploy to EC2
      uses: appleboy/ssh-action@master
      with:
          host: ${{ secrets.HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_PRIVATE_KEY }}
          port: 22
          script: |
              sudo docker pull ${{ env.DOCKER_IMAGE_NAME }}
              CONTAINER_ID=$(sudo docker ps -q --filter "publish=8080-8080")

              if [ ! -z "$CONTAINER_ID" ]; then
                sudo docker stop $CONTAINER_ID
                sudo docker rm $CONTAINER_ID
              fi
              sudo docker run -d -p 8080:8080 ${{ env.DOCKER_IMAGE_NAME }}
```