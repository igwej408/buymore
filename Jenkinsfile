pipeline {
   agent any

   stages{ 
         stage('Cleanup directory'){
            steps{
               script{
                  deleteDir()
               }
            }
         }


       stage('checkout from git repo') {
               steps {
                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/igwej408/buymore.git']])
               }
       }

   
       stage('build docker image') {
              steps{
                  script{
                     sh 'docker build -t igwej408/buymore:1 .'
                     
                }
            }
       }
   
       stage('scan with Trivy'){
         steps{
           sh 'trivy image buymore:1 > trivy-result.txt'
         }
       }
     
       stage('push image to docker hub'){
          steps{
             script{
                withCredentials([string(credentialsId: 'id', variable: 'cred')]) {
                sh 'docker login -u igwej408 -p ${cred}'
}
                sh 'docker push igwej408/buymore:1'
             }

         }
       }


    }
}    