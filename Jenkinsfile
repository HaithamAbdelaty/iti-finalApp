pipeline {
   agent {label 'jenkins-slave'}

   environment {
      DOCKERHUB_CREDENTIALS = credentials('dockerhubcredentials')
   }

   stages {
      
      stage('Build Image'){
         steps {
            sh 'docker build -t haythamabdelaty/haitham-image:latest .'
         }
      }

      stage('Login') {
         steps {
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
         }
      }

      stage('Push Image'){
         steps {
            sh 'docker push haythamabdelaty/haitham-image:latest'
         }
      }

      stage('deploy') {
         steps {
         
               withCredentials([file(credentialsId: 'cluster-config', variable: 'config')]) {
               sh """
                     kubectl apply -f deployment --kubeconfig=${config}
                  """
            }
         }
      }
   }
}