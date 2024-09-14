pipeline{
    agent any
tools{
maven 'maven'
}
stages{

stage('Source Code Checkout '){
    steps{
        git 'https://github.com/God-Father01/PetClinic.git'
    }
}

stage('Build with maven'){
    steps{
        sh 'mvn clean install package'
    }
}




}
}
