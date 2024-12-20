pipeline {
    agent any
    
    environment {
        // Declare environment variables here if needed
        DOCKER_IMAGE = 'my_notes_app'
    }

    stages {
        stage("Clone code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Madhu9209/django-notes-app.git", branch: "main"
            }
        }

        stage('Build') {
            steps {
                echo "Building the Docker image"
                script {
                    sh 'docker --version'
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }
        
        stage("Push to dockerhub") {
            steps {
                echo "Pushing to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubUser")]) {
                    echo "Using DockerHub user: ${env.dockerHubUser}"  // For debugging
                    script {
                        sh """
                            docker tag ${DOCKER_IMAGE} ${env.dockerHubUser}/${DOCKER_IMAGE}:latest
                            echo ${env.dockerHubpass} | docker login -u ${env.dockerHubUser} --password-stdin
                            docker push ${env.dockerHubUser}/${DOCKER_IMAGE}:latest
                        """
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploy the container"
                // Debugging to see the value of dockerHubUser
                echo "DockerHub user is: ${env.dockerHubUser}"  // For debugging
                script {
                    sh """
                        echo "Using Docker image: ${env.dockerHubUser}/${DOCKER_IMAGE}:latest"
                        docker stop my_notes_app || true
                        docker rm my_notes_app || true
                        docker run -d --name my_notes_app -p 8003:8003 ${env.dockerHubUser}/${DOCKER_IMAGE}:latest
                    """
                }
            }
        }
    }
}
