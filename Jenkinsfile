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

    stage('SCM Checkout') { // for display purposes
      // Get some code from a GitHub repository
      git credentialsId: 'git-creds', url: 'https://github.com/srikantpadhi/SpringBoot-Docker-Integration.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.6.1' Maven tool must be configured
      // **       in the global configuration.
      mvnHome = tool 'Maven'
    }

    stage('Build Project') {
      // build project via maven
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
    }

    stage('Build Docker Image') {
      // build docker image
      sh 'docker build -f Dockerfile -t ${dockerImageName}:${env.BUILD_NUMBER} .'
    }

    stage('Deploy Docker Image'){
       echo "Docker Image Tag Name: ${dockerImageTag}"
      // deploy docker image to docker hub
      withCredentials([string(credentialsId: 'docker-pwd', variable:'dockerHubPwd')]){
          sh "docker login -u sp05071983 -p ${dockerHubPwd}"
      }
      sh "docker push ${dockerImageTag}"
    }
}