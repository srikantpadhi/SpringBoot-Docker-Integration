node {
    // reference to maven
    // ** NOTE: This 'Maven' Maven tool must be configured in the Jenkins Global Configuration.
    def mvnHome = tool 'Maven'

    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)

    def dockerRepoUrl = "https://hub.docker.com/repository/docker/sp05071983/myrepo"
    def dockerImageName = "docker-springboot"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"

    stage('1.SCM Checkout') { // for display purposes
      echo "pulling code from git"
      // Get some code from a GitHub repository
      git credentialsId: 'git-creds', url: 'https://github.com/srikantpadhi/SpringBoot-Docker-Integration.git'
      mvnHome = tool 'Maven'
    }

    stage('2.Build Project') {
       echo "building project start"
       bat "${mvnHome}/bin/mvn' clean package"
       echo "building project end"
    }

    stage('3.Build Docker Image') {
     echo "building docker image"
      bat "docker build -f Dockerfile -t ${dockerImageName}:${env.BUILD_NUMBER} ."
      echo"docker Image created successfully"
    }

    stage('4.Deploy Docker Image to Docker Hub'){
       echo "Docker Image Tag Name: ${dockerImageTag}"
       echo "Docker Image Tag Name: ${dockerImageTag}"
       withCredentials([string(credentialsId: 'docker-pwd', variable:'dockerHubPwd')]){
          bat "docker login -u sp05071983 -p ${dockerHubPwd}"
      }
      echo "Push Docker Image into DockerHub"
      bat "docker push ${dockerImageTag}"
    }
}