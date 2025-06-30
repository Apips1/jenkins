pipeline {
   agent none

   stage("Prepare") {
        agent { label "linux && java17" }
        steps {
            echo("Start Job : ${env.JOB_NAME}")
            echo("Build Number : ${env.BUILD_NUMBER}")
            echo("Branch Name : ${env.BRANCH_NAME}")
        }
    }
    stages {
        stage('Build') {
             agent { label "linux && java17" }
            steps {
                script{ 
                for (int i = 0; i < 10; i++) {
                    echo "Script ${i + 1}"
                }
            }
                echo ('Building the project...')
                bat ("./mvnw clean compile test-compile")
                echo ('Build completed successfully.')
            }
        }
        stage('Test') {
             agent { label "linux && java17" }
        steps {

            script {
                def data = [
                    "FirstName": "Afif",
                    "LastName" : "Nugroho"
                ]
                writeJSON(file: 'data.json', json: data)
            }
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
