pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-north-1'
        AWS_ECR_URL = '381492278384.dkr.ecr.eu-north-1.amazonaws.com/ttt'
        ECS_CLUSTER_NAME = 'DevCluster'
        ECS_SERVICE_NAME = 'tttservice'
        TASK_DEFINITION_NAME = 'devtask'
    }

    stages {
        stage('build docker image and update ECR') {
            steps {
                script {
                    docker.build('381492278384.dkr.ecr.eu-north-1.amazonaws.com/ttt')
                    docker.withRegistry('aws-creds') {
                        docker.image('381492278384.dkr.ecr.eu-north-1.amazonaws.com/ttt').push('latest')
                    }
                }
            }
        }
        stage('deploy to ECS'){
            steps {
                withAWS(credentials: 'aws-creds', region: 'eu-north-1') {
                sh 'aws ecs update-service --cluster ${ECS_CLUSTER_NAME} --service ${ECS_SERVICE_NAME} --force-new-deployment'
                }
            }
        }
    }
}
