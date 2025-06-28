pipeline {
    agent { label "linux && java17" }
    stages {
        stage('Build') {
            steps {
                echo 'Hello Build'
                sleep(10)
            }
        }
        stage('Test') {
            steps {
                echo 'Hello Test'
                sleep(10)
            }
        }
        stage('Deploy') {
            steps {
                echo 'Hello Deploy'
                sleep(10)
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
