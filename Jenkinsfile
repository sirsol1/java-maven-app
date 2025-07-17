pipeline {
    agent any

    environment {
        IMAGE_NAME = 'siresol/demo-app:v1.0'
    }

    stages {
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
