#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('test') {
            steps {
                script {9
                    echo "Testing the application..."
                }
            }
        }
        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    def dockerCmd ='docker run -p 3080:3000 -d harikrishnan20010616/react-nodejs-example:1.0'
                    sshagent(['ec2-server-key']) {
                     sh "ssh -o StrictHostKeyChecking=no ec2-user@13.53.134.64 ${dockerCmd}"
                   }
                }
            }
        }
    }
}
