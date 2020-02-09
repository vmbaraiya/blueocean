pipeline {
    agent any
    stages {
      stage('Lint HTML') {
        steps {
          sh 'tidy -q -e *.html'
        }
      stage('Upload to AWS') {
        steps {
          withAWS(region:'us-east-2',credentials:'S3Pipeline') {
            s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'awscodepipeline-blueocean')
          }
        }
      }
