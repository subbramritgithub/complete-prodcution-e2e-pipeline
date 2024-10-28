pipeline{
      agent any
      tools{
            jdk 'java17'
            maven 'maven3'
      }
      environment {
        APP_NAME = "complete-productions-app"
        RELEASE = "1.0.1"
        DOCKER_USER = "subbuengineering"
        DOCKER_PASS = 'docker-login'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials("jenkins-api-token")
      }
      stages{
            stage("cleanup workspace"){
                  steps{
                        cleanWs()
                  }
            }
            stage("checkout to Scm"){
                  steps{
                        git branch: "CICD-DEMO" , credentialsId: "subbramritgithub" , url: "https://github.com/subbramritgithub/complete-prodcution-e2e-pipeline.git"
                  }
            }
            stage("build application"){
                  steps{
                        bat "mvn clean package"
                  }
            }
            stage("test application"){
                  steps {
                        bat "mvn test"
                  }
            }
             stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqube-jenkins-inte') {
                        bat "mvn sonar:sonar"
                    }
                }
            }

        }
            stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                      docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

      
       }
            stage("Trivy Scan") {
            steps {
                script {
		   sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image dmancloud/complete-prodcution-e2e-pipeline:1.0.0-22 --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
                }
            }

        }
      }
}


      
                
      
      
 
      

                 
            

        
          
    
