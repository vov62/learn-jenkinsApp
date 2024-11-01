

pipeline {
    agent any

    environment{
        NETLIFY_SITE_UP = 'e51c0bd5-64cc-4338-a73c-4ff1b9f75af0'
        NETLIFY_AUTH_TOKEN = credentials('netlify_token')
    }

    stages {
        stage('Build') {
            agent{
                docker{
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
        stage('Test'){
            steps{
                sh 'test -f dist/index.html'
            }
        }

        stage('Deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version 
                    echo "Deloying to production, Site ID: $NETLIFY_SITE_UP"
                    node_modules/.bin/netlify link --id $NETLIFY_SITE_UP
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
}
//   docker run -d --name jenkins-from-ubuntu -p 8081:8080 -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker  -v jenkins_home:/var/jenkins_home   jenkins/jenkins:lts
 