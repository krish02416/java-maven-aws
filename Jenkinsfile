#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage('test') {
            steps {
                script {
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
                    'sh docker run -p 3080:3000 -d harikrishnan20010616/react-nodejs-example:1.0'
                 
                }
            }
        }
    }
}
