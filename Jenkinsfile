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
            withCredentials([string(credentialsId: '123456', variable: 'GITHUB_TOKEN')]) {
                sh '''
                    set -x

                    # Configure Git user details
                    git config user.email "godfather77701@gmail.com"
                    git config user.name "${GIT_USER_NAME}"
                    pwd

                    # Replace the image tag in the Deployment.yaml using the workspace variable
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" /God-Father01/PetClinic/manifest/Deployment.yaml

                    # Stage, commit, and push the changes
                    git add /God-Father01/PetClinic/manifest/Deployment.yaml
                    git commit -m "Replace image tag with ${BUILD_NUMBER}"

                    # Push to GitHub (ensure you use the correct branch)
                    #git push https://$GITHUB_TOKEN@github.com/$GIT_USER_NAME/$GIT_REPO_NAME.git HEAD:main
                git push https://$GITHUB_TOKEN@github.com/$GIT_USER_NAME/$GIT_REPO_NAME.git HEAD:master
                '''
            }
        }
    }
}
    }
}

    
