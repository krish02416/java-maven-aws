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
        maven 'Maven'
    }


    stages {
         stage("increment version") {
                    steps {
                        script{
                          echo " Incrementing app version...."
                          sh 'mvn build-helper:parse-version versions:set \
                               -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                                versions:commit'
                          def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                          def version = matcher[0][1]
                          env.IMAGE_NAME= "$version-$BUILD_NUMBER"
                        }
                    }
         }
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
                   sh "bash ./server-cmds.sh ${IMAGE_NAME}"
                }
            }
        }

        stage("commit version update") {
                    steps {
                        script{
                            withCredentials([usernamePassword(credentialsId: 'git-credentials', passwordVariable:'PASS', usernameVariable: 'USER')]){
                                sh 'git config --global user.email "jenkins@example.com"'
                                sh 'git config --global user.name "jenkins"'

                                sh 'git status'
                                sh 'git branch'
                                sh 'git config --list'

                                sh '"git remote set-url origin https://${USER}:${PASS}@github.com/krish02416/java-app-maven.git"'
                                sh 'git add .'
                                sh 'git commit -m "ci: version bump"'
                                sh 'git push origin HEAD:java-8-version'
                            }
                        }
                    }
                }
    }
}
