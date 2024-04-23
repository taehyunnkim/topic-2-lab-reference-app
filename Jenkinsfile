pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'docker build . -t $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/taehyun-app'
            }
        }
        stage('Push Image') {
            steps {
                sh 'aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com'
                sh 'aws ecr describe-repositories --repository-names taehyun-app || aws ecr create-repository --repository-name taehyun-app'
                sh 'docker push $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/taehyun-app'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker pull $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.comtaehyun-app'
                sh 'docker rm -f taehyun-app || true'
                sh 'docker run -d -p "2000:80" --name taehyun-app $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/taehyun-app'
            }
        }
    }
}