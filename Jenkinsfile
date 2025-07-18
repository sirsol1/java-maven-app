pipeline {
    agent any

    environment {
        IMAGE_NAME = 'siresol1/my-repos:v1.0'
        EC2_HOST = 'ec2-user@18.234.239.14'
        APP_PORT = '3080'
        CONTAINER_PORT = '3000'
        CONTAINER_NAME = 'my-repos'
    }

    stages {
        stage('deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        def dockerCmd = """
                            docker stop ${CONTAINER_NAME} || true
                            docker rm ${CONTAINER_NAME} || true
                            docker login -u $DOCKER_USER -p $DOCKER_PASS &&
                            docker pull ${IMAGE_NAME} &&
                            docker run --name ${CONTAINER_NAME} -p ${APP_PORT}:${CONTAINER_PORT} -d ${IMAGE_NAME}
                        """.stripIndent().trim()

                        sshagent(['ec2-server-key']) {
                            sh "ssh -o StrictHostKeyChecking=no ${EC2_HOST} '${dockerCmd}'"
                        }
                    }
                }
            }
        }
    }
}
