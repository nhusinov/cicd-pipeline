pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Check out the code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        // Set port number for main branch
                        port = 3000
                    } else if (env.BRANCH_NAME == 'dev') {
                        // Set port number for dev branch
                        port = 3001
                    } else {
                        // Default port for other branches
                        port = 8080
                    }

                    // Add your build steps here, using the 'port' variable as needed
                    sh "echo 'Port number is $port'"
                    // Add more build and deploy steps as required
                }
            }
        }
    }
}
