pipeline {
    agent none
    environment {
      AUTHOR = 'Afif Nugroho'
      EMAIL = 'lMh4f@example.com'
      WEBSITE = 'https://afifnugroho.com'
    } 

    // triggers {
    //     cron("*/5 * * * *") // Runs every 5 minutes
    // //     pollSCM('*/5 * * * *') // Polls SCM every 15 minutes
    // //     upstream(upstreamProjects: "job1,job2", threshold: hudson.model.Result.SUCCESS) // Triggered by an upstream job
    // // }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')

    }
    parameters {
        string(name: "NAME", defaultValue: "Guest", description: "What is your name")
        text(name: "DESCRIPTION", defaultValue: "Guest", description: "Tell me about yourself")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Need to deploy?")
        choice(name: "SOCIAL_MEDIA", choices: ["Instagram", "Facebook", "Twitter"], description: "Which Social Media")
        password(name: "SECRET", defaultValue: "admin", description: "Encrypt Key")
    }

    stages {
        stage('Parameter'){
            agent { label "linux && java17" }
            steps {
                echo "Hello ${params.NAME}!"
                echo "Description: ${params.DESCRIPTION}"
                echo "Deploy: ${params.DEPLOY}"
                echo "Social Media: ${params.SOCIAL_MEDIA}"
                echo "Secret: ${params.SECRET}"
            }
        }
        stage('Prepare') {
            environment {
                APP = credentials('afif_rahasia')
            }
            agent { label "linux && java17" }
            steps {
                echo "Author : ${env.AUTHOR}"
                echo "Email : ${env.EMAIL}"
                echo "Start Job : ${env.JOB_NAME}"
                echo "Build Number : ${env.BUILD_NUMBER}"
                echo "Branch Name : ${env.BRANCH_NAME}"
                echo "App User: ${env.APP_USR}"
                echo "App Password: ${env.APP_PSW}"
                bat 'echo "App Password: %APP_PSW%" > "rahasia.txt"'
            }
        }
        stage('Build') {
            agent { label "linux && java17" }
            steps {
                script {
                    for (int i = 0; i < 10; i++) {
                        echo "Script ${i + 1}"
                    }
                }
                echo 'Building the project...'
                bat "./mvnw clean compile test-compile"
                echo 'Build completed successfully.'
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
                echo 'Testing the project...'
                bat "./mvnw test"
                echo 'Test completed successfully.'
            }
        }
    }
    stage('Deploy') {
        input {
            message "Do you want to deploy the project?"
            ok "Yes, deploy it!"
            submitter "Apips"
        }
        agent { label "linux && java17" }
        steps {
            echo('Deploying the project...')
            sleep(5)
            echo('Deployment completed successfully.')

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
