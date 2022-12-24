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
                    gv.buildJar()
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    gv.buildImage()
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