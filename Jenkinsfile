pipeline {
    agent { node { label 'Client' } }

    stages {
        stage('Env setup') {
            steps {
                sh 'pip3 install Flask'
                sh 'rm -rf hello-python'
                //sh 'git clone https://github.com/RubanDeventhiran/hello-python'
                git 'https://github.com/RubanDeventhiran/hello-python'
                //error "Setup failed"
            }
        }
        stage('Build-Docker-App') {
            steps {
                sh 'sudo docker build -t python-flask docker/.'
                sh 'sudo docker run -d -p 5001:5000 python-flask:latest'
            }
        }
        stage('Test-Docker-app') {
            steps {
                sh 'curl -Is http://127.0.0.1:5001 | head -n 1'
                //sh 'sudo pkill -9 python3'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'sudo docker ps -aq | sudo  xargs docker stop'
                //sh 'sudo pkill -9 python3'
            }
        }
    }
}
slackSend message: 'Build tested successfully'
