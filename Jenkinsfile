

pipeline {
    agent any

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
    }
}
//   docker run -d --name jenkins-from-ubuntu -p 8081:8080 -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker  -v jenkins_home:/var/jenkins_home   jenkins/jenkins:lts
 