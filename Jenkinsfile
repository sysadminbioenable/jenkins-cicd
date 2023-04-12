pipeline {
    agent any
    environment {
        PROJECT_ID = gps-integrated'
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
                sh '''
                    docker build -t helloworld:$BUILD_NUMBER .
                '''
            }            
        }

        stage ('Tagging & Pushing the image'){
            steps{
                sh '''
                    gcloud auth activate-service-account --key-file=$GCP_SA
                    docker tag helloworld:$BUILD_NUMBER gcr.io/$PROJECT_ID/helloworld:$BUILD_NUMBER
                    docker push gcr.io/$PROJECT_ID/helloworld:$BUILD_NUMBER
                '''
            }
        }

    }
}
