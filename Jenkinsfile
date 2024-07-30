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
        stage('Deploy Image') {
            steps {
                script {
		      sh "ansible-playbook -i ansible/inventory  ansible/deploy-docker.yml"
                    }
                }
            }
        }
        
    }
