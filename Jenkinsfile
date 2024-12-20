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
                    sh 'docker --version' 
                    sh 'docker build -t my_notes_app .' 
                }
            }
        }
        stage("Push to dockerhub") {
            steps {
                echo "Pushing to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerhub_credentials", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubUser")]) {
                    sh '''
                    echo ${dockerHubpass} | docker login -u ${dockerHubUser} --password-stdin
                    '''
                }
            }
        }
    }
}
