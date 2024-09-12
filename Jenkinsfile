pipeline{
agent {
  docker {
    image 'godfather77701/webapp:v49'
  }

environment {
        MY_DOCKER_VAR = "DockerId"
        DOCKER_BUILDNUMBER = "${env.BUILD_NUMBER}"
        //ANOTHER_VAR = 'another-value'
    


Stages{

stage('CHECKOUT FROM SCM'){
steps{
git "https://github.com/God-Father01/PetClinic.git" 
}

stage('BUILD STAGE'){
steps{
script{
sh "mvn clean"
sh "mvn validate"
sh "mvn test"
sh "mvn install"
sh "mvn package"
//all of this can be done in single command also sh"mvn clean package"
}

stage('Containerize with docker'){
steps{
script{
withCredentials([usernameColonPassword(credentialsId: 'DockerId', variable: 'MY_DOCKER_VAR')]) {
    sh "docker build -t godfather77701/webapp:${DOCKER_BUILDNUMBER} ."
    
    sh "docker run -d -p godfather77701/webapp:${DOCKER_BUILDNUMBER}"
}
}
}
}

stage('PUSH to DOCKERHUB'){
steps{
script{
withCredentials([usernameColonPassword(credentialsId: 'DockerId', variable: 'MY_DOCKER_VAR')]) {
    sh "docker push godfather77701/webapp:${DOCKER_BUILDNUMBER}"
}
}
}
}




}
}
}
}
}
}
