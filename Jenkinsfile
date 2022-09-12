pipeline {
agent{
    kubernetes{
        label 'slave'
    }
}
stages{
         stage("Building Application Docker Image"){
            steps{
                script{
                    sh 'gcloud auth configure-docker asia-south1-docker.pkg.dev'
                    sh 'docker build . -t asia-south1-docker.pkg.dev/searce-playground-v1/jenkins/hello-web-app:${BUILD_NUMBER}'
                    }
                }
            }
        stage("Pushing Application Docker Image to Google Artifact Registry"){
            steps{
                script{
                    sh 'docker push asia-south1-docker.pkg.dev/searce-playground-v1/jenkins/hello-web-app:${BUILD_NUMBER}'
        }}}
        stage ("Updating Deployment Manifest") {
            steps {
                script {
                    sh 'sed -i s+asia-south1-docker.pkg.dev/searce-playground-v1/jenkins/hello-web-app:v1+asia-south1-docker.pkg.dev/searce-playground-v1/jenkins/hello-web-app:${BUILD_NUMBER}+g manifests/deployment.yaml'
                }
            }
        }
        stage("Application Deployment on Google Kubernetes Engine"){
            steps{
                script{
                    sh "gcloud container clusters get-credentials app-cluster --zone asia-south1-a --project searce-playground-v1"
                    sh 'kubectl apply -f manifests/.'
                }
            }
        }
    }
    }
