# Deploying a microservice application using docker
In this project, we demonstrate how to deploy a microservice application on docker. For each of the microservice, we will create a dockerfile, then build a docker image which will subsequently be deployed to a docker container.

# Application architecture

![Screenshot (264)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/7a89c96d-ed5d-49f2-90f8-efbdec9caccb)

## Step 1: Creating Dockfiles for the microservices
For each of the microservices, we create a dockerfile which will be used to deploy the image. For the UI web app microservice which is reactjs based, we create the dockerfile using the commands below:

```
FROM node:8
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 8080
CMD ["npm","start"]
```
For the cart microservice which is nodejs based, we create the dockerfile using the commands below:

```
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 1004
CMD ["node","index.js"]
```

For the offers microservice which is java maven based, we generate the dockerfile using the commands below:

```
FROM maven as build
WORKDIR /app
COPY . .
RUN mvn install

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/offers-0.0.1-SNAPSHOT.jar /app
EXPOSE 1001
CMD ["java","-jar","offers-0.0.1-SNAPSHOT.jar"]
```

For the shoes microservice which is also java maven based, we create the dockerfile using the commands below:
```
FROM maven as build
WORKDIR /app
COPY . .
RUN mvn install

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/shoes-0.0.1-SNAPSHOT.jar /app
EXPOSE 1002
CMD ["java","-jar","shoes-0.0.1-SNAPSHOT.jar"]
```
For the wishlist microservice which is python based, the dockerfile is created using the commands below:
```
FROM python:3
COPY . .
RUN pip install flask flask_cors
CMD ["python", "index.py"]
EXPOSE 1003
```
For the zuul-api gateway microservice which java maven based, the dockerfile is created using the commands below:
```
FROM maven as build
WORKDIR /app
COPY . .
RUN mvn install

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/zuul-0.0.1-SNAPSHOT.jar /app
EXPOSE 9999
CMD ["java","-jar","zuul-0.0.1-SNAPSHOT.jar"]
```

## Step 2: Pushing code to repository
From the local dev environment, we push the code to the repository by running the commands below:

```
git status
git add .
git commit -m "added dockerfiles"
git push
```
![Screenshot (265)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/ad43a5fa-4d9c-4482-a95f-cecc3205ff8e)

## Step 3: Assess your repository on your docker host and build docker images
Here, we clone the repository to the server where docker is running and run 'docker build' command on each of the microservice directory where dockerfile is located

To clone the repository, we run the command below:
```
git clone <repository_name>
```
![Screenshot (296)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/fd2d5eac-51b8-444a-be8f-4eadb8908b48)

To build the docker images, run the command below:
```
docker build . -t <microservice_name>
```

![Screenshot (269)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/d3154047-f34f-4c41-b6b1-7d045a1ecd88)

![Screenshot (271)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/e2fdcf30-ad42-406a-89e0-bbd6819498ba)

![Screenshot (272)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/54e2469f-cabf-4982-8463-9a34fe97e1f2)

![Screenshot (273)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/c1016875-acff-443c-952e-cea955714138)

![Screenshot (275)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/c842b186-0520-44f9-a6f9-d6309c184031)

![Screenshot (276)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/0e8fecc0-2cab-4932-b299-a370425f9212)

![Screenshot (277)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/45a590c8-9b65-4a13-bd76-e1d34ff3e6ea)

![Screenshot (278)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/f7eeebf9-e992-4328-9eb3-1a3c787e9ef0)

![Screenshot (279)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/cab35059-bb79-4271-ada6-c16153159332)

![Screenshot (280)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/a25d3a55-a3d0-44c8-847a-dc566a9bee4a)

![Screenshot (282)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/49a57126-538c-4ed6-8b33-a5c5191a0c52)

![Screenshot (283)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/9289fc75-5300-4135-9534-c38dfd3f970c)

## Step 4: Run docker images in containers
Next, we run the created docker images in containers using the command below
```
docker run -d -p<host-port>:<container-port> <image-name>
```
![Screenshot (299)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/feabded8-a496-4a70-8b72-fd2436c42ab7)

![Screenshot (300)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/71a01666-c1a1-4002-b52f-bae4ef9427a7)

## Access the application
To access the application through a web browser, we create an inbound rule to allow traffic on the specified port on the ui microservice

![Screenshot (303)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/b3c398b0-7031-4ad8-818f-b279cd7a6fba)

![Screenshot (298)](https://github.com/kenchuks44/microservices-on-docker/assets/88329191/de4040c2-1b2f-4663-b63d-fd05e3fa5404)


















