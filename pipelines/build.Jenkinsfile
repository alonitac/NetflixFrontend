// pipelines/build.Jenkinsfile

pipeline {
    agent {
       label 'general'
    }
    
    triggers {
        githubPush()
    }
    environment {
        // GIT_COMMIT = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
        // TIMESTAMP = new Date().format("yyyyMMdd-HHmmss")


        IMAGE_TAG = 'v1.0.$BUILD_NUMBER'
        IMAGE_BASE_NAME = 'netflix-app'
        DOCKER_CREDENTIALS = credentials('dockerhub')
        DOCKER_USERNAME = "${DOCKER_CREDENTIALS_USR}"
        DOCKER_PASS = "${DOCKER_CREDENTIALS_PSW}"

        //DOCKER_USERNAME = credentials('dockerhub').username
        //DOCKER_PASS = credentials('dockerhub').password
    }


    stages {
        stage('Docker setup') {
            steps {
                sh '''
                  docker login -u $DOCKER_USERNAME -p $DOCKER_PASS
                '''
            }
        }

        stage('Build & Push') {
            steps {
                sh '''
                  IMAGE_FULL_NAME=$DOCKER_USERNAME/$IMAGE_BASE_NAME:$IMAGE_TAG

                  docker build -t $IMAGE_FULL_NAME .
                  docker push $IMAGE_FULL_NAME
                '''
            }
        }
    }
}
