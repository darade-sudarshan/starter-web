pipeline {
    environment {
        imagename = "daradesudarshan/centralrepo"
        dockerImage = ''
        containerName = 'my-simple-webapp'
        dockerHubCredentials = 'docker'
    }
 
    agent any
 
    stages {
        stage('Cloning Git') {
            steps {
                git([url: 'https://github.com/darade-sudarshan/starter-web.git', branch: 'main'])
            }
        }
 
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build "${imagename}:my-simple-webapp"
                }
            }
        }
 
        stage('Running image') {
            steps {
                script {
                    sh "docker run -dit -p 8084:80 --name ${containerName} ${imagename}:my-simple-webapp /bin/bash"
                    // Perform any additional steps needed while the container is running
                }
            }
        }
 
        stage('Stop and Remove Container') {
            steps {
                script {
                    sh "docker stop ${containerName} || true"
                    sh "docker rm ${containerName} || true"
                }
            }
        }
 
        stage('Deploy Image') {
            steps {
                script {
                    // Use Jenkins credentials for Docker Hub login
                    withCredentials([usernamePassword(credentialsId: dockerHubCredentials, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
 
                        // Push the image
                        sh "docker push ${imagename}:my-simple-webapp"
                    }
                }
            }
        }
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi ${imagename}:my-simple-webapp" 
            }
        } 
    }
}
