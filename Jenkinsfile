pipeline {
    agent{
      label "jenkins-agent"
  }
    tools{
       jdk 'Java17'
       maven 'Maven3'
    }
    stages{
        stage ("clean up workspase"){
           steps{
               cleanWs()
           }

        }
    
        stage ("checkout from scm"){
           steps{
               git branch: 'main', credentialsId: 'github', url: 'https://github.com/Mokhtar-CA/Devops-lab.git'
           }

        }
        stage ("build application"){
           steps{
              sh "mvn clean package"
           }

        }
        stage ("test application"){
           steps{
              sh "mvn test"
           }

        }
        stage ("sonarqube Analysis"){
           steps{
            script {
                sh "curl -v http://sonarqube:9000/api/system/status"
            }
              withSonarQubeEnv('sonarqube-scanner') {
                sh "mvn clean verify sonar:sonar -Dsonar.projectKey=my-project-key -Dsonar.host.url=http://sonarqube:9000"

              }
           }

        }
    }


}