pipeline {
    agent {
        docker {
            image 'maven:3.8.4-jdk-11'  // Maven Docker image
             image 'docker:20.10-dind'
        }
    }

    tools {
        maven 'maven'  // The name you provided when configuring Maven in Jenkins
    }

    environment {
        MY_DOCKER_VAR = "DockerId"
        DOCKER_BUILDNUMBER = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('CHECKOUT FROM SCM') {
            steps {
                git "https://github.com/God-Father01/PetClinic.git"
            }
        }

        stage('BUILD STAGE') {
            steps {
                script {
                    sh "mvn clean"
                    sh "mvn validate"
                    sh "mvn test"
                    sh "mvn install"
                    sh "mvn package"
                }
            }
        }

        stage('Containerize with Docker') {
            steps {
                script {
                    withCredentials([usernameColonPassword(credentialsId: 'DockerId', variable: 'MY_DOCKER_VAR')]) {
                        sh "docker build -t godfather77701/webapp:${DOCKER_BUILDNUMBER} ."
                        sh "docker run -d -p 8080:8080 godfather77701/webapp:${DOCKER_BUILDNUMBER}"
                    }
                }
            }
        }

        stage('PUSH to DOCKERHUB') {
            steps {
                script {
                    withCredentials([usernameColonPassword(credentialsId: 'DockerId', variable: 'MY_DOCKER_VAR')]) {
                        sh "docker push godfather77701/webapp:${DOCKER_BUILDNUMBER}"
                    }
                }
            }
        }
    }
}
