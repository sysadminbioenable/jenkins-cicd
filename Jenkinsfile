pipeline {
    agent any
    environment {
        PROJECT_ID = 'gps-integrated'
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
                bat "cmd   docker tag helloworld:$BUILD_NUMBER gcr.io/$PROJECT_ID/helloworld:$BUILD_NUMBER"
                bat "cmd  docker push gcr.io/$PROJECT_ID/helloworld:$BUILD_NUMBER"
                
            }
        }

    }
}
