pipeline {
   /* agent: The agent directive specifies where the entire Pipeline, or a specific stage, 
      will execute in the Jenkins environment depending on where the agent directive is placed.
   */
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

      /* We will have a production job here that will pause and wait for
         a person to approve the job for the deployment tothe production
         server.  If you want, you can specify:
           input message: 'Approve PRODUCTION deployment?', submmitter: <group_or_individual>
         the submitter will be the only people or person allowed to approve the build deployment
      */
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
