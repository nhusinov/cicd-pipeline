pipeline {
    agent any

    tools {nodejs "node"}
    
    stages {
        stage('Checkout') {
            steps {
                // Check out the code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Install dependencies
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        // Build Docker image for main branch
                        sh 'docker build -t nodemain:v1.0 .'
                    } else if (env.BRANCH_NAME == 'dev') {
                        // Build Docker image for dev branch
                        sh 'docker build -t nodedev:v1.0 .'
                    } else {
                        echo 'No Docker image to build for this branch'
                    }
                }
            }
        }

        stage('Stop and Remove Containers') {
            steps {
                // Stop and remove previously running containers to minimize downtime
                script {
                    sh 'docker stop $(docker ps -q)'
                    sh 'docker rm $(docker ps -a -q)'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        // Run Docker container for main branch
                        sh 'docker run -d --expose 3000 -p 3000:3000 nodemain:v1.0'
                    } else if (env.BRANCH_NAME == 'dev') {
                        // Run Docker container for dev branch
                        sh 'docker run -d --expose 3001 -p 3001:3000 nodedev:v1.0'
                    } else {
                        echo 'No Docker container to run for this branch'
                    }
                }
            }
        }
    }
}
