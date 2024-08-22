pipeline {
    agent any
    environment {
        REPOSITORY = 'registry-server:5000'
        PCC_CONSOLE_URL = "10.160.154.170:8083"
        //In Prisma UTD insert private Prisma Compute example (10.160.154.170:8083) and for Public identity by DNS address (https://us-west1.cloud.twistlock.com/us-4-161026387) under Manage>>System>>Utilities>>Path to Console
        CONTAINER_NAME = "ubuntu"
        // Use Jenkins credentials or environment variables for sensitive data
        TWISTLOCK_TOKEN = credentials('twistlock-token-id')
        // Token found in Prisma under Manage>>System>>Utilities>>>>API token
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
                    docker build -t cam/container:1 .
                    docker image ls
                    '''
                }
            }
        }

        stage('Container Scan') {
            steps {
                script {
                    sh '''
                    curl --progress-bar -L --header "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWRtaW4iLCJyb2xlIjoiYWRtaW4iLCJyb2xlUGVybXMiOltbMjU1LDI1NSwyNTUsMjU1LDI1NSw5NV0sWzI1NSwyNTUsMjU1LDI1NSwyNTUsOTVdXSwic2Vzc2lvblRpbWVvdXRTZWMiOjE4MDAsImlzcyI6InR3aXN0bG9jayIsImV4cCI6MTcyNDM1Mjg0Nn0.3ZyIXGmN_xDwzwybpHsGglH0znAfDtoIK66POp1J86s" https://10.160.154.170:8083/api/v1/util/twistcli > twistcli
                    chmod a+x twistcli
                    ./twistcli images scan --address https://10.160.154.170:8083 --token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWRtaW4iLCJyb2xlIjoiYWRtaW4iLCJyb2xlUGVybXMiOltbMjU1LDI1NSwyNTUsMjU1LDI1NSw5NV0sWzI1NSwyNTUsMjU1LDI1NSwyNTUsOTVdXSwic2Vzc2lvblRpbWVvdXRTZWMiOjE4MDAsImlzcyI6InR3aXN0bG9jayIsImV4cCI6MTcyNDM1Mjg0Nn0.3ZyIXGmN_xDwzwybpHsGglH0znAfDtoIK66POp1J86s --details cam/image:1
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed'
            // Cleanup tasks completed
        }
    }
}

