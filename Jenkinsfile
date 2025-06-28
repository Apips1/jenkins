pipeline {
    agent { label "linux && java17" }
    stages {
        stage('Build') {
            steps {
                echo ('Building the project...')
                bat ("./mvnw clean compile test-compile")
                echo ('Build completed successfully.')
            }
        }
        stage('Test') {
            steps {
                echo ('Testing the project...')
                bat ("./mvnw test")
                echo ('Test completed successfully.')
            }
        }
      
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if the pipeline is successful'
        }
        failure {
            echo 'This will run only if the pipeline fails'
        }
        cleanup {
            echo 'This will run at the end of the pipeline, regardless of success or failure'
        }
    }
}
