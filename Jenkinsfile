pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/rameshkumarvermagithub/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage('Docker Image Scan: trivy '){
            steps{
                sh """   
                 trivy image my-note-app > scan1.txt
                 cat scan1.txt
                 """
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"docker",passwordVariable:"pass",usernameVariable:"user")]){
                sh "docker tag my-note-app rameshkumarverma/my-note-app:latest"
                sh "docker login -u ${user} -p ${pass}"
                sh "docker push rameshkumarverma/my-note-app:latest"
                }
            }
        }
        stage("Deploy with docker-compose"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
