pipeline {
    agent any
    environment {
        PROJECT_ID = 'gps-integrated'
        ARTIFACT_REGISTRY_LOCATION = 'us-central1'
        ARTIFACT_REGISTRY_REPOSITORY = 'my-docker-repo'
        ARTIFACT_REGISTRY_REGISTRY = 'gcr.io'
        SERVICE_NAME = 'test'
        GCP_SA = 'demo-key'
    }
    stages {

        stage('Example') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }

       

        stage ('Tagging & Pushing the image'){
            steps{
                bat "cmd  gcloud auth activate-service-account --key-file=$GCP_SA"
                bat  "cmd docker build -t ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY} ."
                bat "cmd docker push ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY}"
                
            }
        }
         stage('Deploy to Cloud Run') {
            steps {
                script {
                    def gcloud = tool 'google-cloud-sdk'

                    bat "cmd ${gcloud}/bin/gcloud config set project ${PROJECT_ID}"
                    bat "cmd ${gcloud}/bin/gcloud beta run deploy ${SERVICE_NAME} --image ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY} --region ${REGION} --platform managed"
                }
            }
        }

    }
}
