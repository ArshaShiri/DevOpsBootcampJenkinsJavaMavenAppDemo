pipeline {
    agent any
    tools {
        maven 'maven-3.8.6'
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo "incrementing app version..."

                    // This command will update the version in pom.xml
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'

                    // Matcher will contain an array of matched text
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }

        stage('build app') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn clean package'
                    sh 'mvn package'
                }
            }
        }

        stage('build image') {
            steps {
                script {
                    echo "Building the docekr image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t arshashiri/demo-app:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push arshashiri/demo-app:${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    echo 'deploying the application...'
                }
            }
        }

        stage ('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'gitlab-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // We can also direcly do the configuration step in the jenkins server.
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git status'
                        sh 'git config --list'

                        // Set the remote url.
                        sh "git remote set-url origin https://${USER}:${PASS}@git@github.com:ArshaShiri/DevOpsBootcampJenkinsJavaMavenAppDemo.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                    }
                }
            }
        }
    }
}