pipeline {
  agent any
  stages {
    stage('upload to AWS') {
      steps {
        sh '''withAWS(region:\'us-east-2\',credentials:\'aws-static\') {
                 sh \'echo "Uploading content with AWS creds"\'
                     s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:\'index.html\', bucket:\'static-jenkins-pipeline\')
               }'''
        }
      }

    }
  }