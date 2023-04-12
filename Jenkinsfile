pipeline {
    agent any
    environment {
        PROJECT_ID = 'gps-integrated'
        ARTIFACT_REGISTRY_LOCATION = 'us-central1'
        ARTIFACT_REGISTRY_REPOSITORY = 'my-docker-repo'
        ARTIFACT_REGISTRY_REGISTRY = 'gcr.io'
        GCP_SA = 'demo-key'
    }
    stages {

        stage('Example') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }

        stage ('Build') {
            steps {
                bat "cmd docker build -t helloworld:$BUILD_NUMBER ."
            }            
        }

        stage ('Tagging & Pushing the image'){
            steps{
                bat "cmd  gcloud auth activate-service-account --key-file=$GCP_SA"
                bat  "cmd docker build -t ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY} ."
                bat "cmd docker push ${ARTIFACT_REGISTRY_REGISTRY}/${PROJECT_ID}/${ARTIFACT_REGISTRY_REPOSITORY}"
                
            }
        }

    }
}
