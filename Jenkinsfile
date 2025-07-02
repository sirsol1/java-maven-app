
pipeline {
    agent none
    stages {
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                    echo "Executing Pipeline for branch $BRANCH_NAME"
                    echo "integrate Pipeline fo
                }
            }
        }

        stage('build') {
            when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
       
        stage('deploy') {
            when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
            steps {
                
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }
}
