currentBuild.displayName="June-Build-#"+currentBuild.number
pipeline{
    agent any
   options {
      timeout(time: 2, unit: 'MINUTES') 
      }
   
 
    stages{
        stage("Checkout-SCM")
        {
            steps{
              
                cleanWs()
                checkout([$class: 'GitSCM',
                branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false,
                extensions: [[$class: 'CleanBeforeCheckout']], 
                submoduleCfg: [], 
                userRemoteConfigs: [[credentialsId: 'github_credentials', 
                url: 'https://github.com/HariReddy910/June-22.git']]])  
                echo "Download finished form SCM"
             
            }
        
        post{
              failure{
                  script { STAGE_FAILED = "checkout: failed to checkout the application.." }
              }
        }
           }       
             stage("Build "){
               steps {

                sh 'mvn clean package '
                
               
              } 
          
         post{
              failure{
                  script {STAGE_FAILED = " Build : Failed to Build the application.." }
              }  
           }
        }
        stage("archiveArtifacts "){
               steps {

             
                  archiveArtifacts '**/*.war'
                
              } 
           
         post{
              failure{
                  script {STAGE_FAILED = " archiveArtifacts :  archiveArtifacts failed.." }
              }
        }
     }
               
        stage("Deployment-AppServer"){
            steps{
              echo "hi"
             sh label: '', script: 'scp /var/lib/jenkins/workspace/Harindra/webapp/target/webapp.war ubuntu@172.31.2.23:/opt/tomcat9/webapps/june.war'
           }
         post{
              failure{
                  script {STAGE_FAILED = "Deploy Application : failed on application." }
              }
        }
          }
    
          }
}
