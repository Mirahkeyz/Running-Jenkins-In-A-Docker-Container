# Running-Jenkins-In-A-Docker-Container

Scenario: The main objective of this project at Level Up Solutions is to use Docker for creating and managing Jenkins containers, emphasizing the implementation of persistent volumes. This strategy aims to build a robust, scalable Continuous Integration and Continuous Deployment (CI/CD) pipeline, facilitating frequent and reliable deployment of software updates.

# FOUNDATIONAL:

Using the CLI

Pull the Jenkins LTS image from Docker Hub.

Create a Docker volume for Jenkins to persist its data.

Run the container with the volume and correct port mapped to the container.

Retrieve the Admin password.

Login and create a new user.

Launch a new Jenkins container using the same volume and verify that the data has persisted by logging in as the newly created user.

Now that we know what the required tasks are, let’s get started!!!

***For this project, I will be using an Ubuntu EC2 instance that has Docker installed on it. I also will be SSHing into the instance using VS Code***

The very first thing we have to do is pull down the latest Jenkins image from Docker Hub. Head over to command line and type this command :

$ sudo docker pull jenkins/jenkins:lts

![Snipe 1](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/feb7acf1-3da1-414d-bd2e-1de65de0516f)

The second thing we need to do is create the volume:

$ sudo docker volume create jenkins-data-volume

![Snipe 2](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/0dc36779-b64a-4528-8ed6-3f645af5ed84)

The volume has been created. Now we can create the network:

$ sudo docker network create jenkins-network

![Snipe 3](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/91a17d97-8a10-407d-8a58-e3093d90cbad)

The following command will create the container, provide a container name, attach the volume and expose the necessary ports.

$ sudo docker run -d -p 8080:8080 -p 50000:50000 --name jenkins-docker -v jenkins-data-volume:/var/jenkins_home jenkins/jenkins:lts

![Snipe 4](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/aa35986c-e2b6-413c-a94d-f3a4d01b032e)

Please run a sudo docker ps to verify that the container is up and running….

![Snipe 5](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/3a2dc4a1-5940-4926-82ad-34ad0582016f)

Now to see if we can access the Jenkins web interface.

![Snipe 6](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/84767ccc-4402-4f0e-9f26-e92b796c1e66)

Now that Jenkins is up and running within our Docker container, we can head back to the CLI on our Linux host and check the jenkins-docker logs for the admin password.

$ sudo docker logs jenkins-docker

![Snipe 7](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/4967dc3a-4c67-4083-b342-6df8593eb65e)

I entered the password and now I will choose the “Install suggested plugins” option.

![Snipe 8](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/554ca72f-7055-4d9a-a6cf-5c45fd93e9f2)

In the next step we will create a second user.

![Snipe 9](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/1e5bf810-f1e1-42e6-99dc-717e3be18ab8)

After finishing the initial configuration we are greeted with the Jenkins Dashboard.

![Snipe 10](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/db17043f-641a-4984-afdd-3e898eb2f138)

The last step in the Foundational tier is to create a second Jenkins container using the same volume (jenkins-data-volume) and see if the data persists by successfully logging in as the user we just created.

The best way to test data persistence is to stop the first container.

$ sudo docker stop jenkins-docker

![Snipe 11](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/b027abfd-d82e-4aa6-8424-4a3637d092d0)

In the screenshot we can see that the first container is no longer running.

Let’s create the second container:

$ sudo docker run -d -p 8080:8080 -p 50000:50000 --name jenkins-docker-backup -v jenkins-data-volume:/var/jenkins_home jenkins/jenkins:lts

![Snipe 12](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/53c2f99f-6c40-4260-8c4f-c9bf13e9f49f)

The second container is up and running. lets verify that we can access the web interface and log in as the user we created (jenkins-admin).

![Snipe 13](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/0e15f795-e70a-4366-a7fe-2003834a7115)

A good sign that data persisted is that when we try to access the second Jenkins container web interface, we are greeted with a login page and not the configuration page.

![Snipe 14](https://github.com/Mirahkeyz/Running-Jenkins-In-A-Docker-Container/assets/134533695/62a620fe-013d-444e-9269-12f8531411da)

We were able to log into Jenkins via the jenkins-admin account. We have now completed the Foundational tier.













































































