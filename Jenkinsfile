sh """
ssh -o StrictHostKeyChecking=no ec2-user@${EC2_PUBLIC_IP} << EOF
    docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}
    docker pull ${IMAGE_NAME}

    # Check if container is already running
    if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
        echo "Container '${CONTAINER_NAME}' is already running. Skipping docker run."
    else
        # Check if container exists but is stopped
        if [ \$(docker ps -a -q -f name=${CONTAINER_NAME}) ]; then
            echo "Container '${CONTAINER_NAME}' exists but is not running. Starting it..."
            docker start ${CONTAINER_NAME}
        else
            echo "Container '${CONTAINER_NAME}' not found. Running a new one..."
            docker run --name ${CONTAINER_NAME} -p 3080:3000 -d ${IMAGE_NAME}
        fi
    fi
EOF
"""
