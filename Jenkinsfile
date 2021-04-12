pipeline {

  environment {
    registry = "gnishanth444/productsonkubernetes"
    registryCredential = 'docker-creds'
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/justmeandopensource/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
         script {
          withDockerRegistry([ credentialsId: registryCredential,url: ""] ) {
            sh 'docker push gnishanth444/productsonkubernetes":$BUILD_NUMBER"'
          }
      }
    }
    }  

    stage('Deploy App') {
      steps {
        sshagent(['kopskey']){
          sh  "ssh -o StrickHostKeyChecking=no kubernetes-deployment.yaml ubuntu@65.0.100.139:/home/ubuntu/"
          script{
            try{
              sh "ssh ubunut@65.0.100.139 kubectl apply -f ."
            } 
            catch(error){
              sh "ssh ubuntu@65.0.100.139 kubectl create -f ."
            }
          }
        }
      }
    }

  }

 }
  
