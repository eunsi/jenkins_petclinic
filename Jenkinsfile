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
<<<<<<< HEAD
=======
        }
        stage("Push image to gcr") {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE')
                    {
                      withCredentials([file(credentialsId: 'jenkins-gogle', variable: 'GC_KEY')]){
                      sh "cat '$GC_KEY' | docker login -u _json_key --password-stdin https://gcr.io"
                      sh "gcloud auth activate-service-account --key-file='$GC_KEY'"
                      sh "gcloud auth configure-docker"
                      GLOUD_AUTH = sh (
                          script: 'gcloud auth print-access-token',
                         returnStdout: true
                                                ).trim()
                     echo "Pushing image To GCR"
                     sh "docker push asia.gcr.io/exemplary-datum-362307/petclinic:${image-tag}"
>>>>>>> 878c60d27d0fa276d517d8621ac6267b016bcfb6
          }
        stage('Push to registry') {
            steps {
             script {
                    docker.withRegistry('https://asia.gcr.io', 'gcr:exemplary-datum-362307') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
        }        
}
