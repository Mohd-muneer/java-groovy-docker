pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-password' // Docker credentials ID
    }
    stages {
        stage('SCM Checkout') {
            steps {
                // Check out code from the 'main' branch
                git branch: 'main', url: 'https://github.com/Mohd-muneer/java-groovy-docker.git'
            }
        }
        stage('Build') {
            steps {
                // Package the application, skipping tests
                sh 'mvn package -Dmaven.test.skip=true'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'mvn verify'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build Docker image
                def dockerImageName = "mohdmuneer/javadedockerapp_${env.JOB_NAME}:${env.BUILD_NUMBER}"
                sh "docker build -t ${dockerImageName} ."
            }
        }
        stage('Publish Docker Image') {
            steps {
                // Publish Docker image
                withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
                    sh 'docker login -u mohdmuneer -p ${dockerPWD}'
                }
                def dockerImageName = "mohdmuneer/javadedockerapp_${env.JOB_NAME}:${env.BUILD_NUMBER}"
                sh "docker push ${dockerImageName}"
            }
        }
        stage('Run Docker Image') {
            steps {
                // Run Docker image
                def dockerImageName = "mohdmuneer/javadedockerapp_${env.JOB_NAME}:${env.BUILD_NUMBER}"
                def dockerContainerName = "javadockerapp_${env.JOB_NAME}_${env.BUILD_NUMBER}"
                def dockerRun = "sudo docker run -p 8085:8080 -d --name ${dockerContainerName} ${dockerImageName}"
                withCredentials([string(credentialsId: 'deploymentserverpwd', variable: 'dpPWD')]) {
                    sh "${dockerRun}"
                }
            }
        }
    }
}

         
  }
      
