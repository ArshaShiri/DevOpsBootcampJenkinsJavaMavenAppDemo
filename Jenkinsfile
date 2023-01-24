library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/ArshaShiri/DevOpsBootcampJenkinsSharedLibraryDemo.git',
     credentialsId: 'gitlab-credentials'
     ]
)

def gv

pipeline {
    agent any

    tools {
        // We need this to make the maven command availble in our build.
        // The name can be retrieved from Dashboard->Manage Jenkins->Credentials
        maven 'maven-3.8.6'
    }

    environment {
        IMAGE_NAME = 'arshashiri/demo-app:java-maven-2.0'
    }

    stages {
        stage("init") {
            steps{
                script {
                    gv = load "script.groovy"
                }
            }
        }
        
        stage("build jar") {
            steps {
                script {
                    // The same name as the function in Shared Library repository (DevOpsBootcampJenkinsSharedLibraryDemo/vars/buildJar.groovy)
                    buildJar()
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    echo 'building docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to EC2...'
                    def dockerCmd = "docker-compose -f docker-compose.yaml up --detach"

                    sshagent(['ec2-server-key']) {
                        sh "scp docker-compose.yaml ec2-user@3.71.176.75:home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.71.176.75 ${dockerCmd}"
                }
            }
        }
    }
}