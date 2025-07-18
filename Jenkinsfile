pipeline {
    agent any

    environment {
        IMAGE_NAME = 'siresol/demo-app:v1.0'
        EC2_HOST = 'ec2-user@18.234.239.14'
    }

    stages {
        stage('deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        def dockerLogin = "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                        def dockerCmd = "docker pull ${IMAGE_NAME} && docker run -p 3000:3000 -d ${IMAGE_NAME}"
                        def remoteCommand = "${dockerLogin} && ${dockerCmd}"

                        sshagent(['ec2-server-key']) {
                            sh "ssh -o StrictHostKeyChecking=no ${EC2_HOST} '${remoteCommand}'"
                        }
                    }
                }
            }
        }
    }
}
