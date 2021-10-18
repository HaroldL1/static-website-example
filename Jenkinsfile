pipeline{
    environment{
        IMAGE_NAME = "HaroldL1/static-website-example"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "static-website-example"
        STAGING = "project-staging"
        PRODUCTION = "project-production"
        USERNAME = "HaroldL1"
       

    }


    agent { label "projet" }

    stages {
        stage("build"){
            steps {
                sh """
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                """
            }
        }
        stage("run"){
            steps{
                sh """
                   docker run --name ${CONTAINER_NAME} -d -p 8000:80 -e PORT=8000 ${IMAGE_NAME}:${IMAGE_TAG}
                   sleep 5
                """
            }
        }

        stage ('test'){
            agent any
            steps{
                script{
                    sh '''
                       curl http://172.17.0.1 | grep -q "Dimension"
                    '''
                }
            }
        }

        stage('clean') {
            agent any
            steps {
                script {
                    sh '''
                       docker stop ${CONTAINER_NAME}
                       docker rm ${CONTAINER_NAME}
                    '''
                }
            }
        }

        stage('push') {
            agent any
            steps {
                script {
                    sh '''
                       docker login -u ${USERNAME} -p ${PASSWORD}
                       docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
}
