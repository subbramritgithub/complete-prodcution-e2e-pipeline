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
                        git: 'branch = CICD-DEMO' , 'credentailsid = subbramritgithub' , 'url = https://github.com/subbramritgithub/complete-prodcution-e2e-pipeline.git'
                  }
            }
      
      }
}
      
                
      
      
 
      

                 
            

        
          
    
