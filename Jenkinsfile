pipeline {
    agent any

    environment {
        PROJECT_ID = 'gps-integrated'
        ARTIFACT_REGISTRY_LOCATION = 'us-central1'
        ARTIFACT_REGISTRY_REPOSITORY = 'my-docker-repo'
        ARTIFACT_REGISTRY_REGISTRY = 'gcr.io'
        GCP_SA='demo-key'
    }

   stages {
        stage('Authenticate with Artifact Registry') {
            steps {
                bat "cmd gcloud auth configure-docker ${ARTIFACT_REGISTRY_REGISTRY}"
            }
        }

        stage('Build and push Docker image') {
            steps {
                bat "cmd gcloud auth activate-service-account --key-file=$demo-key"
                bat  "cmd docker build -t ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY} ."
                bat "cmd docker push ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY}"
            }
        }

        stage('Pull Docker image from Artifact Registry') {
            steps {
                bat "cmd docker pull ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY}"
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
