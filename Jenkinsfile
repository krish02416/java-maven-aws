#!/user/bin/env groovy

library identifier: "jenkins-shared-library@master", retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/krish02416/jenkins-shared-library.git',
     credentialsId: 'git-credentials'])

pipeline {   
    agent any
    tools {
        maven 'Maven'
    }
    environment{
        IMAGE_NAME = 'harikrishnan20010616/demo-app:1.0' 
    }       
    stage("build jar") {
       steps {
            script{
                echo 'Building application jar'
                buildJar()
            }
        }
    }
    stage("build image") {
            steps {
                script{
                    echo 'Building the docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)

                }
            }
    }
    
    stage("deploy") {
             steps {
                script {
                    def dockerCmd ="docker run -p 8080:8080 -d ${IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                     sh "ssh -o StrictHostKeyChecking=no ec2-user@16.171.193.216 ${dockerCmd}"
                   }
                }
            }
    }               
}
