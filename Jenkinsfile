pipeline {
    agent any

    environment {
        IMAGE_NAME = 'siresol/demo-app:v1.0'
    }

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
                    sh "docker build -t ${IMAGE_NAME} ."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    def dockerCmd = "docker pull ${IMAGE_NAME} && docker run -p 3000:3000 -d ${IMAGE_NAME}"
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@18.234.239.14 '${dockerCmd}'"
                    }
                }
            }
        }
    }
}
