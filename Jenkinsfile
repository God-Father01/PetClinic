pipeline {
    agent {
        docker {
            image 'godfather77701/webapp:v49'
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
                    // Or use a single command: sh "mvn clean package"
                }
            }
        }

        stage('Containerize with Docker') {
            steps {
                script {
                    withCredentials([usernameColonPassword(credentialsId: 'DockerId', variable: 'MY_DOCKER_VAR')]) {
                        sh "docker build -t godfather77701/webapp:${DOCKER_BUILDNUMBER} ."
                        sh "docker run -d -p 8080:8080 godfather77701/webapp:${DOCKER_BUILDNUMBER}"  // Fixed port mapping
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
