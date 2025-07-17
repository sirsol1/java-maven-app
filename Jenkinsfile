pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                echo "Testing the application..."
            }
        }

        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -p 3000:3000 -d siresol/demo-app:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.239.14 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
