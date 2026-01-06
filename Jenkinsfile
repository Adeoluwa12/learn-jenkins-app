

pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd8cc7608-3150-46b5-96d8-1dbcdd32808c'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install -g netlify-cli
                   netlify --version
                   echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                   netlify status
                '''
            }
        }
    }



    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
