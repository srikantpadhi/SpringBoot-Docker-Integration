node {
    def mvnHome = tool 'Maven'
    def dockerImageName = ""
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
      dockerImageName = bat "docker build -f Dockerfile -t sp05071983/myrepo:${env.BUILD_NUMBER} ."
      echo"*********docker Image created successfully********"
    }

    stage('4.Deploy Docker Image to Docker Hub'){
       echo "********Login to DockerHub*********"
       //withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'mycreds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']])
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')])
        {
           echo "uname=${dockerHubUse}r pwd=${dockerHubPassword}"
           bat "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
      }
      echo "*******Push Docker Image ${dockerImageName} into DockerHub*********"
      bat "docker push sp05071983/myrepo:${dockerImageName}"
      echo "*******Docker Image pushed to DockerHub Successfully******"
    }

}
