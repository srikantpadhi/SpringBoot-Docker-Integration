pipeline {
    enviornment{
        mvnHome = tool 'Maven'
        dockerRepoUrl = "https://hub.docker.com/repository/docker/sp05071983/myrepo"
        dockerImageName = "docker-springboot"
        dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    }
    agent any  
    stages {
      stage('1.SCM Checkout') { 
          steps{
                echo "*********pulling code from git*********"
                git credentialsId: 'git-creds', url: 'https://github.com/srikantpadhi/SpringBoot-Docker-Integration.git'
                echo "*********Code pulled from git repository successfully*********"
               }
            }

    stage('2.Build Project') {
        steps{
               echo "************building project start***********"
               bat "${mvnHome}/bin/mvn clean package"
              echo "********Project build Successfully***********"
            }
         }

    stage('3.Build Docker Image') {
         steps{
               echo "********start building docker image************"
               bat "docker build -f Dockerfile -t ${dockerImageName}:${env.BUILD_NUMBER} ."
               echo"*********docker Image created successfully********"
              }
         }

    stage('4.Deploy Docker Image to Docker Hub'){
        steps{
               echo "********Docker Image Tag Name: ${dockerImageTag}***********"
               echo "********Login to DockerHub*********"
               withCredentials([usernameColonPassword(credentialsId: 'docker-pwd', variable:'dockerHubPwd')]){
                              echo "${dockerHubPwd}"
                              bat "docker login -u sp05071983 -p ${dockerHubPwd}"
                             }  
            
      echo "*******Push Docker Image into DockerHub******"
      bat "docker push ${dockerImageTag}"
      echo "*******Docker Image pushed to DockerHub Successfully******"
     }
   }
  }
}
