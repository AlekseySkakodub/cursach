pipeline {
    agent {
        node {
            label 'client 1' 
        }
    }
    parameters {
        text(name: 'INDEX_HTML_CONTENT', description: 'Paste index.html content here')
    }
    stages {
        stage('Preparation') {
            steps {
                git credentialsId: 'gitea-password', 
                url: 'http://10.10.40.7:3000/aleksey/course_project',
                branch: 'main' 
            }
        }
        stage('Check') {
            steps {
                echo 'Check important app directory'
                sh "sudo find ./* -name go-rest-api"
            }
        }
        stage('Change index.html') {
            steps {
                echo 'Change index.html'
                script {
                    writeFile file: './go-rest-api/html/index.html', text: params.INDEX_HTML_CONTENT
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Build docker image'
                sh "cd ./go-rest-api && sudo docker build -t go-app:1.3 ."
                echo 'Run docker compose'
                sh "cd ./go-rest-api && sudo docker compose up -d"
            }
        }
    }
}