pipeline {
    agent any

    environment {
       NETLIFY_SITE_ID = '0b313306-026d-48e7-a154-b1cfc3e7fba6'
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
         steps { sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                ''' }
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
                                npm install netlify-cli
                                node_modules/.bin/netlify --version
                                echo "Deploying to Netlify..."
                                 node_modules/.bin/netlify status
                            '''
                        }
                    }

    }

}
