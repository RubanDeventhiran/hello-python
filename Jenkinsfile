pipeline {
    agent { node { label 'Client' } }

    stages {
        stage('Pre-cleanup') {
            steps {
                sh 'sudo docker ps -aq | sudo  xargs docker stop'
                //sh 'docker container rm $(docker container ls -aq)'
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
                sh 'sudo docker run -d -p 5000:5000 python-flask:latest'
                script{def response = sh(script: 'curl http://127.0.0.1:5000', returnStdout: true)}
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
            archiveArtifacts artifacts: 'docker/app/', onlyIfSuccessful: true
        }
    }
}
slackSend message: 'Build tested successfully'
