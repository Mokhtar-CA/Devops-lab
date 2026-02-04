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
    }


}