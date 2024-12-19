pipeline{
    agent any
    
    stages{
        stage("Clone code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/Madhu9209/django-notes-app.git", branch: "main"
            }
        }
        stage('Build') {
            steps {
                echo "Building the Docker image"
                script {
                    // Ensure Docker is installed and available
                    sh 'docker --version' // Optional, check Docker version
                    sh 'docker build -t my_notes_app .' // Build the Docker image
                }
            }
        }
    }
}
