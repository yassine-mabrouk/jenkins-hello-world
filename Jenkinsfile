pipeline {
    agent any
    tools {
        maven "M399"
    }
    stages {
        stage('Build') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/yassine-mabrouk/jenkins-hello-world-.git'
                sh "mvn clean package -DskipTests=true"
            }
        }
        stage('Unit Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('Deploy') {
            steps {
                sh """
                    # Kill any existing Java process on port 6767
                    lsof -ti:6767 | xargs kill -9 || true
                    
                    # Start the application
                    java -jar target/hello-demo-*.jar &
                    
                    # Wait for application to start (adjust sleep time as needed)
                    sleep 30
                    
                 
                """
            }
        }
        stage('Integration Testing') {
            steps {
                sh "curl -s http://localhost:6767/hello"
            }
        }
    }
    post {
        always {
            // Cleanup: Kill the Java process
            sh 'lsof -ti:6767 | xargs kill -9 || true'
        }
    }
}
