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
   }
}
