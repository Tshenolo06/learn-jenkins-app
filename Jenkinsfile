pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '50abd799-ba5d-4bec-a4c9-4ce67132578a'
        NETLIFY_AUTH_TOKEN = credentiials('netlify-token')
    }

    stages {
        /*

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
                    ls -la
                '''
            }
        }
        */

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    #test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E') {
             agent {
                docker {
                     image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                     
                }
            }
 
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test --reporter=html
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
                    npm install netlify-cli
                    npm install netlify-cli@20.1.1
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        } 
        
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}

