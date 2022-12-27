@Library('jenkins-shared-library')
def gv

pipeline {
    agent any

    tools {
        // We need this to make the maven command availble in our build.
        // The name can be retrieved from Dashboard->Manage Jenkins->Credentials
        maven 'maven-3.8.6'
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
                    buildImage("arshashiri/demo-app:jma-3.0")
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}