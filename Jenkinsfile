pipeline {
    agent any 
    tools { 
        maven 'maven'
        nodejs 'node'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                sh "git clone https://github.com/NouranZouabi/deploy-app-spring-angular.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("deploy-app-spring-angular/angular-app"){
                    sh "docker build -t nouran10/myapp-frontend . --no-cache"
		    sh "docker push nouran10/myapp-frontend"
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("deploy-app-spring-angular/springboot/app"){
                      sh "mvn clean install"
                      sh "docker build -t nouran10/myapp-backend . --no-cache"
		      sh "docker push nouran10/myapp-backend"
                  }                
              }
          }
        stage('Run Sonarqube') {
            environment {
                scannerHome = tool 'lil-sonar-tool';
            }
            steps {
              withSonarQubeEnv(credentialsId: 'sonar', installationName: 'lil sonar installation') {
                sh "${scannerHome}/bin/sonar-scanner"
              }
            }
        }

        stage ("Run docker compose") {
            steps {
                 dir("app"){
		    sh "docker compose down --volumes" 
		    sh "docker compose pull"
                    sh "docker compose up -d"
                }                
            }
        }
    }
}
