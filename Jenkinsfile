node {
    def mvnHome = tool 'Maven'
    def dockerRepoUrl = "https://hub.docker.com/repository/docker/sp05071983/myrepo"
    def dockerImageName = "docker-springboot"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"

    stage('1.Checkout SourceCode') { 
      echo "*********pulling code from git*********"
      git credentialsId: 'git-creds', url: 'https://github.com/srikantpadhi/SpringBoot-Docker-Integration.git'
      mvnHome = tool 'Maven'
      echo "*********Code pulled from git repository successfully*********"
    }

    stage('2.Compile and Build Project') {
       echo "************building project start***********"
       bat "${mvnHome}/bin/mvn clean package"
       echo "********Project build Successfully***********"
    }

    stage('3.Create Docker Image') {
     echo "********start building docker image************"
      bat "docker build -f Dockerfile -t ${dockerImageName}:${env.BUILD_NUMBER} ."
      echo"*********docker Image created successfully********"
    }

    stage('4.Deploy Docker Image to Docker Hub'){
       echo "********Docker Image Tag Name: ${dockerImageTag}***********"
       echo "********Login to DockerHub*********"
       withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'mycreds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]){
           echo "uname=$USERNAME pwd=$PASSWORD"
           bat "docker login -u ${USERNAME} -p ${PASSWORD}"
      }
      docker tag ubuntu-xenial-mymicro karlcode/ubuntu-xenial-mymicro:latest
      echo "*******Push Docker Image into DockerHub******"
      bat "docker push ${dockerImageName}"
      echo "*******Docker Image pushed to DockerHub Successfully******"
    }

}
