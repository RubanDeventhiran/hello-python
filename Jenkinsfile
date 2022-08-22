pipeline {
    agent { node { label 'Client' } }

    stages {
        stage('Pre-cleanup') {
            steps {
                sh 'sudo docker ps -aq | sudo  xargs docker stop'
            }
        }
        stage('Env setup') {
            steps {
                git 'https://github.com/RubanDeventhiran/hello-python'
                //error "Setup failed"
            }
        }
        stage('Build-Docker-App') {
            steps {
                sh 'sudo docker build --pull -t python-flask docker/.'
            }
        }
        stage('Test-Docker-app') {
            steps {
                sh 'sudo docker run -d -p 5001:5000 python-flask:latest'
                sh 'curl -Is http://127.0.0.1:5001 | head -n 1'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'sudo docker ps -aq | sudo  xargs docker stop'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'app/main.py', onlyIfSuccessful: true
        }
    }
}
slackSend message: 'Build tested successfully'
