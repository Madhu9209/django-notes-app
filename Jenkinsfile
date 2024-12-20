pipeline {
    agent any
    
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
                    sh 'docker build -t my_notes_app .' 
                }
            }
        }
        
        stage("Push to dockerhub") {
            steps {
                echo "Pushing to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubUser")]) {
                    echo "Using DockerHub user: ${env.dockerHubUser}"  // For debugging
                    sh """
                    docker tag my_notes_app ${env.dockerHubUser}/my_notes_app:latest
                    echo ${env.dockerHubpass} | docker login -u ${env.dockerHubUser} --password-stdin
                    docker push ${env.dockerHubUser}/my_notes_app:latest
                    """
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploy the container"
                sh """
                echo "Using Docker image: ${env.dockerHubUser}/my_notes_app:latest"
                docker stop my_notes_app || true
                docker rm my_notes_app || true 
                docker run -d --name my_notes_app -p 8000:8000 ${env.dockerHubUser}/my_notes_app:latest
                """
            }
        }
    }
}
