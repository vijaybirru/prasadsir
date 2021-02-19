node{
  stage('Git Checkout'){
sh "rm -rf $WORKSPACE/*"
git 'https://github.com/vijaybirru/hello-world-spring-boot.git'
  }
  stage('MVN Package'){
    def mvnHome = tool name: 'maven-3', type: 'maven'
    sh "${mvnHome}/bin/mvn clean package"
  }
  stage('Image creation'){
  
    sh "docker build -t springbootimage-${BUILD_NUMBER} ."
  }
  stage('remove old container & images'){
  
 echo "previous build number: ${currentBuild.previousBuild.getNumber()}"
 sh " docker rmi -f springbootimage-${currentBuild.previousBuild.getNumber()} "
 sh " docker rm  -f springbootcontainer-${currentBuild.previousBuild.getNumber()} "

sh " docker images"
  }
 stage('Deployment '){
  
    sh "docker run -d --name springbootcontainer-${BUILD_NUMBER} -p 8090:8080 springbootimage-${BUILD_NUMBER}:latest"
  }
}
