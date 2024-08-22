pipeline {
    agent any
    environment {
        REPOSITORY = 'registry-server:5000'
        PCC_CONSOLE_URL = "10.160.154.170:8083"
        CONTAINER_NAME = "ubuntu"
        // Use Jenkins credentials or environment variables for sensitive data
        TWISTLOCK_TOKEN = credentials('twistlock-token-id')
        DOCKER_REGISTRY_TOKEN = credentials('docker-registry-token-id')
    }
    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }      

        stage('Build') {
            steps {            
                script {
                    sh ''' 
                    docker build -t $REPOSITORY/$CONTAINER_NAME:$BUILD_NUMBER .
                    docker image ls
                    '''
                }
            }
        }

        stage('Container Scan') {
            steps {
                script {
                    sh '''
                    curl --progress-bar -L --header "Authorization: Bearer ${TWISTLOCK_TOKEN}" https://us-west1.cloud.twistlock.com/us-4-161026387/api/v1/util/twistcli > twistcli
                    chmod a+x twistcli
                    ./twistcli images scan --address ${PCC_CONSOLE_URL} --token ${TWISTLOCK_TOKEN}
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed'
            // Optionally, perform cleanup tasks here
        }
    }
}

