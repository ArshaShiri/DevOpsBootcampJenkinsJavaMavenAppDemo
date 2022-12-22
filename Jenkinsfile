def gv

pipeline {
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: 'version to deploy on')
        booleanParam(name: 'executeTests', defaultValue: true, description:'')
    }

    stages {
        // Import groovy script.
        stage("init") {
            steps{
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage("build") {
            steps{
                script {
                    gv.buildApp()
                }
            }
        }

        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps{
                script {
                    gv.testApp()
                }
            }
        }

        stage("deploy") {
            input {
                message "Select the environment to deploy to"
                ok "Environment selected"
                parameters {
                    choice(name: 'ENV', choices: ['dev', 'staging', 'production'], description: 'environment to deploy on')
                }
            }
            steps{
                script {
                    gv.deployApp()
                    echo "Deploying to ${ENV}"
                }
            }
        }
    }
}