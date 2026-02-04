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
    }
    stages{
        stage ("checkout from scm"){
           steps{
               git branch: 'main', credentialsId: 'github', url: 'https://github.com/Mokhtar-CA/Devops-lab.git'
           }

        }
    }


}