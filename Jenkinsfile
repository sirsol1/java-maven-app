pipeline {
    agent any

    environment {
        IMAGE_NAME = 'siresol1/my-repos:v1.0'
        EC2_HOST = 'ec2-user@18.234.239.14'
        APP_PORT = '3080'
        CONTAINER_NAME = 'my-repos'
    }

    stages {
        stage('deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        def dockerCmd = """
                            docker ps -q --filter 'publish=${APP_PORT}' | xargs -r docker stop
                            docker ps -a -q --filter 'publish=${APP_PORT}' | xargs -r docker rm
                            docker login -u $DOCKER_USER -p $DOCKER_PASS &&
                            docker pull ${IMAGE_NAME} &&
                            docker run --name ${CONTAINER_NAME} -p ${APP_PORT}:${APP_PORT} -d ${IMAGE_NAME}
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
