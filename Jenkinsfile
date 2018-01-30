pipeline {
   agent any
   stages{
      stage('Build') {
         steps {
            sh '/usr/local/maven/bin/mvn clean package'
         }
         post {
            success {
                echo 'Now archiving ...'
                archiveArtifacts artifacts: '**/target/*.war'
            }
         }
      }

      stage ('Deploy to staging') {
         steps {
            build job: 'deploy-to-staging'
         }
      }

      stage ('Deploy to production') {
         steps {
            timeout(time:5, unit:'DAYS') {
               input message: 'Approve PRODUCTION deployment?'
            }
            
            build job: 'deploy-to-prod'
         }
         post {
            success {
               echo 'Code deployed to production'
            }

            failure {
               echo 'Deployment failed'
            }
         }
      }
   }
}
