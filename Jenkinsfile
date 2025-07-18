pipeline {
    agent any

    environment {
        DOCKER_USER = 'siresol1'
        DOCKER_PASS = credentials('docker-hub-repo') // Jenkins credential ID
        IMAGE_NAME = 'siresol1/my-repos:v1.0'
        CONTAINER_NAME = 'my-repos'
        HOST_PORT = '3080'
        CONTAINER_PORT = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def EC2_PUBLIC_IP = '18.234.239.14'

                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@${EC2_PUBLIC_IP} << EOF
                        docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}
                        docker pull ${IMAGE_NAME}

                        # Check if container is running
                        if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                            echo "Container ${CONTAINER_NAME} is already running. Skipping docker run."
                        else
                            # Check if container exists but stopped
                            if [ \$(docker ps -a -q -f name=${CONTAINER_NAME}) ]; then
                                echo "Starting stopped container ${CONTAINER_NAME}..."
                                docker start ${CONTAINER_NAME}
                            else
                                echo "Running new container ${CONTAINER_NAME}..."
                                docker run --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} -d ${IMAGE_NAME}
                            fi
                        fi
                    EOF
                    """
                }
            }
        }
    }
}
