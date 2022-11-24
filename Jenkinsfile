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
        
    stages {
        stage('Build image') {
            steps {
                script {
                    app = docker.build("exemplary-datum-362307/fatclinic")
                }
            }
        }
        
        stage("Push image to gcr") {
            steps {
                script {
                    docker.withRegistry('https://asia.gcr.io', 'gcr:exemplary-datum-362307') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }

        stage('K8S Manifest Update') {

            steps {

                git credentialsId: 'jenkins', 
                    url: 'https://github.com/eunsi/argo_petclinic.git',
                    branch: 'main'

                sh "sed -i 's/petclinic:.*\$/petclinic:${env.BUILD_NUMBER}/g' spring-boot.yaml"
                sh "git add spring-boot.yaml"
                sh "git commit -m '[UPDATE] petclinic ${env.BUILD_NUMBER} image versioning'"

                withCredentials([gitUsernamePassword(credentialsId: 'jenkins')]) {
                    sh "git push -u origin main"
                }
            }
            post {
                    failure {
                    echo 'Update failure !'
                    }
                    success {
                    echo 'Update success !'
                    }
            }
        }

    }             
  }
}
