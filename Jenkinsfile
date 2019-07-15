pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage("Github pull") {
            agent {
                docker { image "node:11-alpine" }
            }
            steps {
                git branch: "master",
                    credentialsId: "ForGithubJenkTraining",
                    url: "https://github.com/ForGithubJenkTraining/react-app-jenk.git"
                sh "ls -lah"
            }
        }
        stage("node install") {
            agent {
                docker { image "node:11-alpine" }
            }
            steps {
                sh "npm install"
            }
        }
        stage("node test") {
            agent {
                docker { image "node:11-alpine" }
            }
            steps {
                sh "npm test"
            }
        }
        stage("node build") {
            agent {
                docker { image "node:11-alpine" }
            }
            steps {
                sh "npm run build"
            }
        }
        stage("docker build") {
            steps {
                sh "docker build . -t react-nginx"
            }
        }
        stage('delete previous stack') {
            steps {
                sh "docker stack rm react-nginx-stack"
            }
        }
        stage("deploy new stack") {
            steps {
                sh "docker stack deploy -c docker-compose.yml react-nginx-stack"
            }
        }
    }
    post {
        always {
          sh 'echo "This will always run"'
        }
      }
}
