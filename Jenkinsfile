pipeline {
    agent any 

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Clean up code') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout using SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true  // Correct placement
                }
            }

            steps {
                sh '''
                    ls -l
                    node --version
                    npm --version
                    npm install
                    npm run build
                    ls -l
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true  // Correct placement
                }
            }

            steps {
                sh '''
                    npm run test
                '''
            }
        }


        stage('Deploy on vercel') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true  // Correct placement
                }
            }


            steps {
                sh '''
                    npm install -g vercel
                '''
            }
        }
    }
}
