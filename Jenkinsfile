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
        stage('Push to registry') {
            steps {
                script {
                def build_time = new Date()
                def sdf = new SimpleDateFormat("yyyyMMddHHmm")
               // docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-jhwangdemo') {
                docker.withRegistry('https://gcr.io', 'gcr:pa-jhwang') {
                dockerImage.push("${sdf.format(build_time)}-${env.BUILD_NUMBER}")
                dockerImage.push("latest")
      }
    }
  }
}
        }        
}
