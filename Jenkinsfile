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
                    docker.withRegistry('https://us.gcr.io', 'gcr:<gcp-project-id>') {
                        app.push("${commit_id}")
                        app.push("latest")
        }
                }
            }
        }
        
    }
}
