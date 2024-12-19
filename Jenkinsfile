pipeline{
    agent any
    
    stages{
        stage("Clone code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/Madhu9209/django-notes-app.git", branch: "main"
            }
        }
    }
}
