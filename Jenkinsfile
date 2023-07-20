pipeline {
  agent any
  stages {
    stage('Build and push Docker image') {
      steps {
        withCredentials(bindings: [file(credentialsId: 'test', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
          bat "cmd /c gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
          bat 'cmd /c gcloud auth configure-docker --quiet'
          bat 'cmd /c gcloud auth configure-docker asia.gcr.io'
        }

        bat "cmd /c docker build -t ${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME} ."
        bat "cmd /c docker push ${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME}"
      }
    }

    stage('Deploy to Cloud Run') {
      steps {
        bat """ cmd /c
                  gcloud run deploy ${SERVICE_NAME} \
                    --image=${REGISTRY_HOSTNAME}/nisarg-cludrun/${IMAGE_NAME} \
                    --platform=managed \
                    --region=${REGION} \
                    --project="nisarg-cludrun"
                    --allow-unauthenticated
                """
      }
    }

  }
  environment {
    PROJECT_ID = 'nisarg-cludrun'
    SERVICE_NAME = 'test'
    REGION = 'us-central1'
    IMAGE_NAME = 'myimage'
    REGISTRY_HOSTNAME = 'asia.gcr.io'
  }
}