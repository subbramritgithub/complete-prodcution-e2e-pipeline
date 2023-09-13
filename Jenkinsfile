pipeline{
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "subbuengineering"
        DOCKER_PASS = 'docker-token'
        IMAGE_NAME = "${subbuengineering}" + "/" + "${docker-jenkins}"
        IMAGE_TAG = "${RELEASE}-${7155243}"
        JENKINS_API_TOKEN = credentials("jenkins-api-token")

    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/dmancloud/complete-prodcution-e2e-pipeline'
            }

        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

        stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
        
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqube') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',docker-token) {
                        docker_image = docker.build "${suthpick}"
                    }

                    docker.withRegistry('',docker-token) {
                        docker_image.push("${RELEASE}")
                        docker_image.push('latest')
                    }
                }
            }

        }
            }
}

                 
            

        
          
    
