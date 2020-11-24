pipeline {
    agent any
    stages {
        stage ('checkout'){
            steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/vbronfman/igentify_test.git']]])
            }
        }
        stage('build') {
            steps {
                sh 'echo "Hello world from Jenkins within Docker job of ${BUILD_NUMBER} ${JOB_NAME} ${BUILD_DISPLAY_NAME}" > $(pwd)/index.html'
                sh 'chmod 777 $(pwd)/index.html'
                sh 'docker kill $(docker ps -l -q) || echo Ok'
                sh 'docker run -d --rm -v $(pwd):/simplehttp -p 10000:10000 python:3.8 python -m http.server 10000 --directory /simplehttp'
                sh 'docker cp $(pwd)/index.html $(docker ps -l -q):/simplehttp'
                 
                
            }
        }
    }
}
