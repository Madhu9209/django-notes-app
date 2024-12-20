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
            sh "docker run -d --name my_notes_app -p 8003:8003 ${imageName}"
        }
    }
}
