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
        stage('minikube')
        environment{
                GIT_REPO_NAME="PetClinic"
                GIT_USER_NAME="God-Father01"
            }
        {
            steps{
                script{
                    withCredentials([usernameColonPassword(credentialsId: 'Githubtoken', variable: 'GIT_REPO_NAME')]) {
                        sh '''
                        git conig user.email "godfather77701@gmail.com"
                        git congig user.name "God-Father01"
                        DOCKER_IMAGE_BUILD_ID = "${BUILD_NUMBER}"
                        sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" PetClinic/tree/master/manifest/Deployment.yaml
                        git add PetClinic/tree/master/manifest/Deployment.yaml
                        git commit -m "replace imagetag"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_REPO_NAME } HEAD:main


                        '''
}

                   
                   
                   
                    
                }
            }
        }


    }
}
