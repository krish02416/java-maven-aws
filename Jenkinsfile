#!/usr/bin/env groovy

library(
    identifier: "jenkins-shared-library@master",
    retriever: modernSCM([
        $class: 'GitSCMSource',
        remote: 'https://github.com/krish02416/jenkins-shared-library.git',
        credentialsId: 'git-credentials'
    ])
)

pipeline {
    agent any

    tools {
        maven 'maven-3.9'
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
                    def dockerCmd = "docker run -p 8080:8080 -d ${IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@16.171.193.216 '${dockerCmd}"
                    }
                }
            }
        }
    }
}
