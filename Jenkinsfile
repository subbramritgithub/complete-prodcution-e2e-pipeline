pipeline{
      agent any
      tools{
            jdk 'java17'
            maven 'maven3'
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

        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-jenkins-inte'
                }
            }

        }
      
      }
}

      
                
      
      
 
      

                 
            

        
          
    
