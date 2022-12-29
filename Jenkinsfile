pipeline {
    agent any
    tools {
        maven 'maven-3.8.6'
    }
    stages {
        // Executing for all the branches.
        stage('build app') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn package'

                }
            }
        }

        stage('build image') {
            steps {
                script {
                    echo "Building the docekr image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t arshashiri/demo-app:jma-2.0 .'
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh 'docker push arshashiri/demo-app:jma-2.0'
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                sccript {
                    echo 'deploying the application...'
                }
            }
        }
    }
}