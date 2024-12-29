pipeline {
  agent any
  tools {
    maven 'Maven_3_8_7'
  }

  stages {
    stage('CompileandRunSonarAnalysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          bat("mvn -Dmaven.test.failure.ignore verify sonar:sonar -Dsonar.login=$SONAR_TOKEN -Dsonar.projectKey=easybuggy -Dsonar.host.url=http://localhost:9000/")
        }
      }
    }
    stage('Build') {
      steps {
        withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
          script {
            app = docker.build("warsiviews123/testeb")
          }
        }
      }
    }
    stage('RunContainerScan') {
      steps {
        withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
          script {
            try {
              bat("C:\\devops\\snyk\\snyk-win.exe  container test warsiviews123//testeb")
            } catch (err) {
              echo err.getMessage()
            }
          }
        }
      }
    }
    stage('RunSnykSCA') {
      steps {
        withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
          bat("mvn snyk:test -fn")
        }
      }
    }
    stage('RunDASTUsingZAP') {
      steps {
        bat("C:\\devops\\zap\\ZAP_2.15.0_Crossplatform\\ZAP_2.15.0\\zap.sh -port 9393 -cmd -quickurl https://www.example.com -quickprogress -quickout C:\\devops\\zap\\ZAP_2.15.0_Crossplatform\\ZAP_2.15.0\\Output.html")
      }
    }

   stage('Debug') {
    steps {
        bat "echo %PATH%"
        // bat "where \"python\""
        bat "\"C:\\Users\\Maaz Amjad\\AppData\\Local\\Programs\\Python\\Python310\\python\" --version"
    }
}



    // stage('checkov') {
    //   steps {
    //     bat("checkov -s -f main.tf")
    //   }
    // }

  }
}
