pipeline {
    agent any
    environment {
        DOCKER_IMAGE_BUILD_ID = "${BUILD_NUMBER}"
    }
    tools {
        maven 'maven'
    }
    stages {
        stage('Source Code Checkout') {
            steps {
                git 'https://github.com/God-Father01/PetClinic.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install package'
            }
        }

        stage('Docker') {
            steps {
                script {
                    // Fetch Docker credentials from Jenkins
                    withCredentials([usernamePassword(credentialsId: 'DockerId', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Build the Docker image
                        sh "docker build -t godfather77701/webapp:${BUILD_NUMBER} ."
                        
                        // Log in to Docker Hub
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                        
                        // Push the Docker image
                        def dockerImageName = "godfather77701/webapp:${BUILD_NUMBER}"
                        sh "docker push ${dockerImageName}"
                    }
                }
            }
        }
        stage('minikube'){
            steps{
                script{
                    sh "minikube start --driver=docker"
                  sh "cd manifest/" 
                  sh " sed -i 's/image: godfather77701\/webapp:.*/image: godfather77701\/webapp:'"${BUILD_NUMBER}"'/' Deployment.yaml"
                  
                  sh "kubectl apply -f Deployment.yaml"
                  sh "kubectl apply -f Service.yaml"
                  sh "kubectl apply -f ingress.yaml"
                  sh "minikube addons enable ingress"

                   
                   
                   
                    
                }
            }
        }


    }
}
