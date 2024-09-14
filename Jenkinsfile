pipeline {
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
                    withCredentials([usernameColonPassword(credentialsId: 'DockerId', variable: 'DOCKER_IMAGE_BUILD_ID')]) {
                        // Build the Docker image
                        sh "docker build -t godfather77701/webapp:${BUILD_NUMBER} ."
                        // Push the Docker image which is built with build number tag
                        def dockerImageName = "godfather77701/webapp:${BUILD_NUMBER}"
                        // Push the Docker image
                        sh "docker push ${dockerImageName}"
                    }
                }
            }
        }
    }
}
