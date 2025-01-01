pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package -DskipTests=true'
        archiveArtifacts 'target/hello-demo-*.jar'
      }
    }

    stage('Unit Test') {
      steps {
        sh 'mvn test'
        junit(testResults: 'target/surefire-reports/TEST-*.xml', keepProperties: true, keepTestNames: true)
      }
    }

    stage('Deploy') {
      steps {
        sh '''
                    # Kill any existing Java process on port 6767
                    lsof -ti:6767 | xargs kill -9 || true

                    # Start the application
                    java -jar target/hello-demo-*.jar &

                    # Wait for application to start (adjust sleep time as needed)
                    sleep 30


                '''
      }
    }

    stage('Integration Testing') {
      steps {
        sh 'curl -s http://localhost:6767/hello'
      }
    }

    stage('End') {
      steps {
        echo 'End of my pipline '
      }
    }

  }
  tools {
    maven 'M399'
  }
  post {
    always {
      sh 'lsof -ti:6767 | xargs kill -9 || true'
    }

  }
}
