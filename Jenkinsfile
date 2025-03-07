pipeline{
	agent any
	environment{
		DOCKER_IMAGE = "santhosh2010/my-jenkins-app"
		DOCKER_CREDENTIALS_ID = "docker4"

	}
	stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning repository..."pipeline {

    agent any

    environment {
        DOCKER_IMAGE = "my-jenkins-app"
        DOCKER_TAG = "latest"
        DOCKER_REPO = "santhosh2010/my-jenkins-app"
        DOCKER_CREDENTIALS_ID = "docker4" // Jenkins credentials ID
        CONTAINER_NAME = "mycontainer1"
        CONTAINER_NAME1 = "mycontainer2"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Santhosh2010-ramesh/jenkins-ssh-docker.git'
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Run Container Locally') {
            steps {
                script {
                    sh """
                        # Stop and remove existing container if running
                        docker ps -a -q --filter name=${CONTAINER_NAME} | xargs -r docker stop || true
                        docker ps -a -q --filter name=${CONTAINER_NAME} | xargs -r docker rm || true

                        # Run new container
                        docker run -d -p 8092:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_REPO}:${DOCKER_TAG}"
                        sh "docker push ${DOCKER_REPO}:${DOCKER_TAG}"
                    }
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    sh """
                        sshpass -p "root" ssh -o StrictHostKeyChecking=no master@192.168.203.128 << "EOF"
docker pull karthik449/my-jenkins-app:latest
docker ps -a -q --filter name=mycontainer2 | xargs -r docker stop || true
docker ps -a -q --filter name=mycontainer2 | xargs -r docker rm || true
docker run -d -p 80:80 --name mycontainer2 karthik449/my-jenkins-app:latest
EOF

                    """
                }
            }
        }

    }
}
                git url: 'https://github.com/Santhosh2010-ramesh/jenkins-ssh-docker.git', branch: 'main'
                echo "Repository cloned successfully."
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Starting Docker build..."
                sh "docker build -t ${DOCKER_IMAGE} ."
                echo "Docker image built successfully."
            }
        }

	stage('Run Container Locally'){
		steps {
			script {
				sh "docker run -d -p 8081:80 --name jenkins-docker ${DOCKER_IMAGE}:latest"
			}
		}
}

        stage('Push Docker Image') {
            steps {
                script {
                    echo "Logging into Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-creds') {
                        echo "Pushing Docker image..."
                        sh "docker push ${DOCKER_IMAGE}"
                        echo "Docker image pushed successfully."
                    }
                }
            }

        }
        stage('Deploy to server') {
            steps {
                script {
withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
sh '''
echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
docker pull kpkm25/my-jenkins-app:latest
'''


}

                }
            }
        }

    }
}
