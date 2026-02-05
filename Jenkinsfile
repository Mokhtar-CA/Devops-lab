pipeline {
    agent {
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
        dockerTool 'Docker'
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "111218"
        DOCKER_PASS = 'dockerhub'
        // Corrected string concatenation syntax
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    }
    stages {
        stage("clean up workspace") {
            steps {
                cleanWs()
            }
        }
    
        stage("checkout from scm") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Mokhtar-CA/Devops-lab.git'
            }
        }

        stage("build application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("test application") {
            steps {
                sh "mvn test"
            }
        }

        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqubecred') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqubecred'
                }
            }
        }

        stage("Build & Push Docker Image") {
            steps {
                script {
                    // Combined registry blocks for cleaner execution
                    docker.withRegistry('', DOCKER_PASS) {
                        def docker_image = docker.build("${IMAGE_NAME}")
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}