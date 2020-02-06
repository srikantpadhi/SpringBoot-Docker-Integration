node {
    def mvnHome = tool 'Maven'
    def dockerImageName = "docker-springboot"
    stage('1.Checkout SCM') { 
      echo "*********pulling code from git*********"
      git credentialsId: 'git-creds', url: 'https://github.com/srikantpadhi/SpringBoot-Docker-Integration.git'
      echo "*********Code pulled from git repository successfully*********"
    }

    stage('2.Compile') {
       echo "************building project start***********"
       bat "${mvnHome}/bin/mvn clean package"
       echo "********Project build Successfully***********"
    }

    stage('3.Build Docker Image') {
      echo "********start building docker image************"
        bat "docker build -f Dockerfile -t sp05071983/myrepo:${dockerImageName}-${env.BUILD_NUMBER} ."
      echo"*********docker Image created successfully********"
    }

    stage('4.Deploy To Docker Hub'){
       echo "********Login to DockerHub*********"
       withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'mycreds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
        {
           echo "uname=${USERNAME}r pwd=${PASSWORD}"
           bat "docker login -u ${USERNAME} -p ${PASSWORD}"
      }
      echo "*******Push Docker Image ${dockerImageName}-${env.BUILD_NUMBER} into DockerHub*********"
      bat "docker push sp05071983/myrepo:${dockerImageName}-${env.BUILD_NUMBER}"       
    }
    //stage('5. Run Docker Container in Dev Env') {
   //      echo "********Login to DockerHub*********"
   //    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'mycreds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
   //     {
   //        echo "uname=${USERNAME}r pwd=${PASSWORD}"
   //        bat "docker login -u ${USERNAME} -p ${PASSWORD}"
   //   }
   //   echo "****Running docker image****"
   //   bat "docker run -p 8090:8090 ${dockerImageName}-${env.BUILD_NUMBER}" 
   // }

   
}
