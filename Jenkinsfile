pipeline {
    agent any
    environments {
        CONTAINER_NAME = 'myapp'
        IMAGE_NAME = 'node-backend'
        EC2_IP = 'ubuntu@34.201.229.103'
        BRANCH = 'main'
    }

    stages {

        stage('git checkout') {
            steps {
                git branch: "${BRANCH}",
                credentialsId: 'eflow',
                url: 'https://github.com/sunandgmohan/nodedemobackend.git'
            }
        }
        stage('deploy to ec2') {
            steps {
                sshagent(['EC2_CRED']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${EC2_IP}'
                    set -e
                    mkdir -p /home/ubuntu/backend
                    cd /home/ubuntu/backend
                    if [ ! -d ".git" ]; then
                       git clone -b ${BRANCH} https://github.com/sunandgmohan/nodedemobackend.git .
                    else 
                       git fetch origin ${BRANCH}
                       git reset --hard origin/${BRANCH}
                    fi
                    docker rm -f ${CONTAINER_NAME} || true
                    docker rmi ${IMAGE_NAME} || true

                    docker build -t ${IMAGE_NAME} .

                    docker run -d -p 3000:3000 \
                    --name ${CONTAINER_NAME} \
                    ${IMAGE_NAME}
                    """

        
            }
        }

        
    }
}


