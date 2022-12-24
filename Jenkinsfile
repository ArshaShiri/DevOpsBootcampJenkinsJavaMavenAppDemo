pipeline {
    agent none
    stages {
        // Executing for all the branches.
        stage('test') {
            steps {
                echo "Testing the application..."
                echo "Executing pipeline for the branch $BRANCH_NAME"
            }
        }

        stage('build') {
            when {
                expression {
                    // Running for main branch only.
                    // Env variable that is for multibranch pipelines.
                    BRANCH_NAME == 'main'
                }
            }
            steps {
                echo "Building the application..."
            }
        }

        stage('deploy') {
            when {
                expression {
                    // Running for main branch only.
                    // Env variable that is for multibranch pipelines.
                    BRANCH_NAME == 'main'
                }
            }
            steps {
                echo "Deploying the application..."
            }
        }
    }
}