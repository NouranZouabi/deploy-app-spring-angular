pipeline {
  agent any
  
  tools{
    maven 'maven'
    nodejs 'node'
  }

  stages{
    stage("clean up"){
      steps {
        deleteDir()
      }
    }
    stage("clone repo"){
      steps {
      sh "git clone https://github.com/NouraneZouabi/deploy-app-spring-angular.git"
      }
    }

    stage("Generate backend image"){
      steps {
       dir("deploy-app-spring-angular/springboot/app"){
        sh "mvn clean package"
        sh "usermod -aG docker jenkins"
        sh "docker build -t nouran10/springboot-app . --no-cache"
        sh "docker push nouran10/springboot-app"
       }
      }
    }

  }
}
