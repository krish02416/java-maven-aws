#!/usr/bin/env groovy

library(
    identifier: "jenkins-shared-library@ssh-script",
    retriever: modernSCM([
        $class: 'GitSCMSource',
        remote: 'https://github.com/krish02416/jenkins-shared-library.git',
        credentialsId: 'git-credentials'
    ])
)

pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        IMAGE_NAME = 'harikrishnan20010616/demo-app:1.0'
    }

    stages {

        stage('Build JAR') {
            steps {
                script {
                    echo 'Building application jar'
                    buildJar()
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building the Docker image'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                echo 'deploying docker image to EC2'
                sh 'docker-compose -f docker-compose.yaml up -d'
                }
            }
        }
    }
}
