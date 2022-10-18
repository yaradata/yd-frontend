pipeline {
    agent any 

    environment {
        frontend = "${WORKSPACE}"
        docker_image = "authentication"
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
                        def customImage = docker.build("yaradata/$docker_image:latest")
                        customImage.push()

                        // docker push yaradata/authentication:tagname

                        // docker tag local-image:tagname new-repo:tagname
                        // docker push new-repo:tagname
                    }

                }
            }
        }
        
        stage('Kubernetes Deploy') {
            steps{
                echo "deploy" 
                sh "docker run -itd -p 4116:3000 --name frontend yaradata/$docker_image:latest"
            }
        }
    }
    
}

