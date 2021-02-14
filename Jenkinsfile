// pipeline {
//     agent any

//     tools { nodejs "node-js"}
    
//     stages {
//         stage('Build') {
//             steps {
//                 sh 'npm install'
//             }
//         }
//         stage('Test') {
//             steps {
//                 sh 'npm test'
//             }
//         }
//         stage('Deliver') {
//             steps {
//                 sh 'npm run build'
//                 sh 'npm start &'
//                 input message: 'Finished using the web site? (Click "Proceed" to continue)'
//             }
//         }
//     }
// }


pipeline {
  environment {
    imageName = "simply-node-js-react-npm-app"
    registryCredential = 'dockerhub-credentials'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Initialize') {
        def dockerHome = tool 'docker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
    stage('Building image') {
      steps {
        script{
          dockerImage = docker.build imageName
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( 'https://hub.docker.com', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
            dockerImage.push('latest')

          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:latest"
      }
    }
  }
}