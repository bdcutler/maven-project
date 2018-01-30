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
   }
}
