pipeline {
    agent {
        docker {
            image 'docker:20.10-dind'  // Docker-in-Docker image
            args '-v /var/lib/docker'  // Volume for Docker daemon
        }
    }
tools{
    maven 'maven'
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
                    // Install Maven in the Docker container
                    sh '''
                    apt-get update
                    apt-get install -y maven
                    '''
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
