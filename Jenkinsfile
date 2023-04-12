pipeline {
    agent any

    environment {
        PROJECT_ID = 'gps-integrated'
        IMAGE = 'gcr.io/gps-integrated/us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage'
        REGION = 'us-central1'
        SERVICE_NAME = 'test'
    }

    stages {
        stage('Build and push container image') {
            steps {
                script {
                    docker.withRegistry('https://gcr.io', 'gcr-auth') {
                        def dockerfile = "Dockerfile"
                        def buildArgs = "--build-arg some_argument=value"
                        def image = docker.build("${IMAGE}:${env.BUILD_NUMBER}", "-f ${dockerfile} ${buildArgs} .")
                        image.push()
                    }
                }
            }
        }

        stage('Deploy to Cloud Run') {
            steps {
                script {
                    def gcloud = tool 'google-cloud-sdk'

                    bat "cmd ${gcloud}/bin/gcloud config set project ${PROJECT_ID}"
                    bat "cmd ${gcloud}/bin/gcloud beta run deploy ${SERVICE_NAME} --image ${IMAGE}:${env.BUILD_NUMBER} --region ${REGION} --platform managed"
                }
            }
        }
    }
}
