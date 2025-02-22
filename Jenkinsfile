pipeline {
    agent any 

    environment {
        NODE_ENV = 'development'
        VERCEL_TOKEN = credentials('VERCEL_TOKEN')
    }
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
                    reuseNode true
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
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "Installing test dependencies"
                    npm install --only=dev
                    echo "Running tests"
                    npm run test
                '''
            }
        }

        stage('Deploy on vercel') {
            agent {
                docker {
                    image 'node:22.11.0-alpine3.20'
                    args '-u root'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install -g vercel
                    vercel --prod --token=$VERCEL_TOKEN --confirm --name=react_github_action
                '''
            }
        }
    }
}