// //dckr_pat__snbNu8AUrQA_pX-9FWkz7UYTCw
// pipeline {
//     agent any
//     environment {
//         DOCKER_CREDENTIAL = credentials('DOCKER_CREDENTIAL')
//     }
//     stages {
//         stage('Stage 1: Building the WAR file and docker image') {
//             steps {
//                 script {
//                     checkout scm
//                     sh 'rm -rf *.war'
//                     sh 'jar -cvf ROOT.war -C src/main/webapp . '
//                     sh 'echo ${BUILD_TIMESTAMP}'
//                     sh 'docker login -u bidhanjanit -p ${DOCKER_CREDENTIAL}'
//                     def customImage = docker.build("bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}")
//                     sh "docker push bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}"
//                 }
//             }
//         }
//     }
// }

pipeline {
  agent any
  environment {
    DOCKER_REGISTRY = "docker.io"
    DOCKER_USERNAME = "bidhanjanit"
    DOCKER_PASSWORD = "dckr_pat__snbNu8AUrQA_pX-9FWkz7UYTCw"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'rm -rf *.war'
        sh 'jar -cvf ROOT.war -C src/main/webapp .'
      }
      post {
        success {
          archiveArtifacts artifacts: '*.war', fingerprint: true
        }
      }
    }
    stage('Docker Build and Push') {
      steps {
        script {
          docker.withRegistry("${DOCKER_REGISTRY}", "docker-credentials") {
            def image = docker.build("bidhanjanit/swe-assignment2:${BUILD_NUMBER}")
            image.push()
            sh 'docker tag bidhanjanit/swe-assignment2::${BUILD_NUMBER} ${DOCKER_REGISTRY}/bidhanjanit/swe-assignment2:${BUILD_NUMBER}'
            withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
              sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${DOCKER_REGISTRY}"
              sh "docker push ${DOCKER_REGISTRY}/bidhanjanit/swe-assignment2:${BUILD_NUMBER}"
            }
          }
        }
      }
    }
  }
}
