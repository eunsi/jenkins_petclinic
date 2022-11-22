pipeline {
    agent  {
        label 'dind-agent'
    }
    stages {
        stage("Build") {
            steps {
                script {
                    sh "./mvnw clean install"
                }
            }
        }
        stage("Build image") {
            steps {
                script {
                    app = docker.build("exemplary-datum-362307/petclinic")
                }
            }
        }
        stage("Push image to gcr") {
            steps {
                script {
                    docker.withRegistry('https://gcr.io', 'gcr:My First Project') {
                        app.push("${env.BUILD_NUMBER}")
        }
                }
            }
        }
        
    }
}
