pipeline{
agent any{
stages{
stage('git clone'){
echo "cloning the git repoo"
sh "git clone https://github.com/God-Father01/Pet.git"
}
stage('build'){

echo "Buliding the artifact using Maven"
sh "mvn clean package"

}
stage('Try Docker'){
echo "Tod Pod "
}
}
}
}
