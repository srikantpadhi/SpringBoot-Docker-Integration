node {
    def mvnHome = tool 'Maven'
    def dockerImageName = "docker-springboot"
    stage('1.Checkout SourceCode') { 
      echo "*********pulling code from git*********"
      git credentialsId: 'git-creds', url: 'https://github.com/srikantpadhi/SpringBoot-Docker-Integration.git'
      echo "*********Code pulled from git repository successfully*********"
    }

    stage('2.Compile and Build Project') {
       echo "************building project start***********"
       bat "${mvnHome}/bin/mvn clean package"
       echo "********Project build Successfully***********"
    }

    stage('3.Create Docker Image') {
      echo "********start building docker image************"
        bat "docker build -f Dockerfile -t sp05071983/myrepo:${dockerImageName}-${env.BUILD_NUMBER} ."
      echo"*********docker Image created successfully********"
    }

    stage('4.Deploy Docker Image to Docker Hub'){
       echo "********Login to DockerHub*********"
       withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'mycreds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
        {
           echo "uname=${USERNAME}r pwd=${PASSWORD}"
           bat "docker login -u ${USERNAME} -p ${PASSWORD}"
      }
      echo "*******Push Docker Image ${dockerImageName}-${env.BUILD_NUMBER} into DockerHub*********"
      bat "docker push sp05071983/myrepo:${dockerImageName}-${env.BUILD_NUMBER}"
      echo "*******Docker Image pushed to DockerHub Successfully******"
    }
    stage('5.Run Docker Container in Dev enviornment'){
        echo "****Running docker image****"
        bat "docker run -p 8090:8090 sp05071983/myrepo/${dockerImageName}-${env.BUILD_NUMBER}"
    }

}
