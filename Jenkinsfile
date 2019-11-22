node{
   stage('SCM Checkout'){
   git credentialsId: '8a7cca70-c4a2-475b-8867-27c64e83fac4', url: 'https://github.com/lakshmikar23/my-app.git'
   }
    stage('Mvn Package'){
     def mvnHome = tool name: 'maven-3.5.3', type: 'maven'
     def mvnCMD = "${mvnHome}/bin/mvn"
     sh "${mvnCMD} clean package"
   }
       stage('Build Docker Image'){
       sh 'docker build -t 5611/my-app:2.0.0 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
        sh "docker login -u 5611 -p ${dockerHubPwd}"
     }
     sh 'docker push kammana/my-app:2.0.0'
   }
   stage('Run Container on Dev Server'){
     def dockerRun = 'docker run -p 8089:8080 -d --name my-app 5611/my-app:2.0.0'
     sshagent(['dev-server']) {
       sh "ssh -o StrictHostKeyChecking=no ubuntu@18.216.146.61 ${dockerRun}"
     }
   }
}
