pipeline {
    agent any 

    environment {
        frontend = "${WORKSPACE}"
        docker_image = "yaradata/frontend"
    }

    stages {
        stage('Docker Build') {
            steps{
                // build docker image 
                sh "docker build -t $docker_image ."
            }
        } 

        stage('Push image to docker hub') {
            steps {
                script {
                    
                    docker.withRegistry('', 'dockerHub-access' ) {
                        def customImage = docker.build("$docker_image:latest")
                        customImage.push()
                    }

                }
            }
        }
        
        stage('Kubernetes Deploy') {
            steps{
                echo "deploy" 
                sh "docker run -itd -p 4000:3000 --name frontend $docker_image:latest"
            }
        }
    }
    
}

