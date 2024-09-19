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

        stage('Minikube') {
            environment {
                GIT_REPO_NAME = "PetClinic"
                GIT_USER_NAME = "God-Father01"
            }
            steps {
                script {
                   withCredentials([string(credentialsId: 'GithubWebapp', variable: ' GIT_USER_NAME')]) {
    
                       sh '''
                        git config user.email "godfather77701@gmail.com"
                        git config user.name "God-Father01"
                        pwd

                        
                        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" /var/lib/jenkins/workspace/Petclinic/manifest/Deployment.yaml


                        git add /var/lib/jenkins/workspace/Petclinic/manifest/Deployment.yaml
                        git commit -m "Replace image tag with ${BUILD_NUMBER}"
                        git push https://${GIT_USER_NAME}:${GithubWebapp}@github.com/God-Father01/PetClinic HEAD:master
                        '''
                    }
                }
            }
        }
    }
}
