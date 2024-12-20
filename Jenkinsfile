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

        stage('Deploy') {
            steps {
                echo 'Deploy the container'
                script {
                    def dockerHubUsername = 'madhu220'  // Ensure this is your correct DockerHub username
                    def imageName = "${dockerHubUsername}/my_notes_app:latest"
                    echo "Using Docker image: ${imageName}"
                    
                    // Stop and remove the existing container if it's running
                    sh "docker stop my_notes_app || true"
                    sh "docker rm my_notes_app || true"
                    
                    // Run the new container with the correct image reference
                    sh "docker run -d --name my_notes_app -p 8000:8000 ${imageName}"
                }
            }
        }
    }
}
