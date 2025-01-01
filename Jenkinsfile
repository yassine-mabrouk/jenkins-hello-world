
pipeline {
  agent any
  stages {
    stage('Maven Version') {
          steps {
            sh 'echo Print Maven Version'
            sh 'mvn -version'
          }
        }
    stage('Show Parameters') {
      steps {
        sh "echo Branch Name: ${params.BRANCH_NAME}, Sleep Time: ${params.SLEEP_TIME}, App Port: ${params.APP_PORT}"
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts 'target/hello-demo-*.jar'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }

    stage('Containerization') {
      steps {
        sh 'echo Docker Build Image..'
        sh 'echo Docker Tag Image....'
        sh 'echo Docker Push Image......'
      }
    }

    stage('Kubernetes Deployment') {
      steps {
        sh 'echo Deploy to Kubernetes using ArgoCD'
      }
    }

    stage('Integration Testing') {
      steps {
          sh "sleep ${params.SLEEP_TIME}"
        sh 'echo Testing using cURL commands......'
      }
    }
  }
  tools {
    maven 'M399'
  }
}
