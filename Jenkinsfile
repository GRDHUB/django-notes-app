pipeline
{
    agent any
    stages{
        stage("clone-code"){
            steps{
                echo "Clone the code from source"
                git url: "https://github.com/GRDHUB/django-notes-app" , branch: "main"
            }
        }
        stage("build"){
            steps{
                echo "Build the docker image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push-to-dockerHub"){
            steps{
                echo "Push dockerimage to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "dockerHubUser", passwordVariable: "dockerHubPass")]){
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest  "
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploy Your containers"
                sh "docker-compose down && docker-compose up -d"
            }  
    
        }
    }
}
