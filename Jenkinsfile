pipeline {
    agent none

    environment {
        AUTHOR = 'Afif Nugroho'
        EMAIL = 'lMh4f@example.com'
        WEBSITE = 'https://afifnugroho.com'
    }

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
        stage("OS Setup") {
            matrix {
                axes {
                    axis {
                        name 'OS'
                        values 'linux', 'windows', 'macos'
                    }
                    axis {
                        name 'ARC'
                        values '32', '64'
                    }
                }
                excludes{
                    exclude {
                    axis {
                        name 'OS'
                        values 'macos'
                    }   
                    axis {
                        name 'ARC'
                        values '32'
                    }
                    }
                }
                stages {
                    stage("OS Setup") {
                        agent {
                            node {
                                label "linux && java17"
                            }
                        }
                        steps {
                            echo "Running setup for OS=${OS}, ARC=${ARC}"
                        }
                    }
                }

            }
        }

        stage('Preparation') {
            parallel {
                stage("Prepare Java") {
                    agent { label "linux && java17" }
                    steps {
                        echo "Preparing Java environment..."
                        sleep(5)
                    }
                }
                stage("Prepare Maven") {
                    agent { label "linux && java17" }
                    steps {
                        echo "Preparing Maven environment..."
                        sleep(5)
                    }
                }
            }
        }

        stage('Parameter') {
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
                bat 'echo "App Password: $APP_PSW" > rahasia.txt'
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
                bat './mvnw clean compile test-compile'
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
                bat './mvnw test'
                echo 'Test completed successfully.'
            }
        }

        stage('Deploy') {
            input {
                message "Do you want to deploy the project?"
                ok "Yes, deploy it!"
                submitter "Apips"
                parameters {
                    choice(name: 'TARGET_ENV', choices: ['Development', 'Staging', 'Production'], description: 'Select the environment to deploy')
                }
            }
            agent { label "linux && java17" }
            steps {
                echo "Deploying to ${params.TARGET_ENV}"
            }
        }

        stage('Release') {
            when {
                expression { return params.DEPLOY }
            }
            agent { label "linux && java17" }
            steps {
             withCredentials([usernamePassword(
                credentialsId: 'afif_rahasia',
                usernameVariable: 'USER',
                passwordVariable: 'PASSWORD'
             )]){
                bat ('echo "Release it with -u ${USER} -p ${PASSWORD}" > "release.txt"')
             }
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
