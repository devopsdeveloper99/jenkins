# jenkins

1. Install jenkins using docker image from DockerHub
2. Install AWS CLI inside docker
- docker exec -it -u root jenkins-docker bash
- apt-get update && apt-get install -y curl unzip
- curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install
- aws --version
3. Install AWS Cred plugins etc
jenkins script to test AWS connectivity:
example 1:
pipeline {
    agent any
    environment{
        AWS_DEFAULT_REGION="us-east-1"
    }

    stages {
        stage('Hello') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-creds', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws --version
                        aws ec2 describe-instances
                    '''
                }

            }
        }
    }
}

example 2:
pipeline {
    agent any
    environment{
        AWS_DEFAULT_REGION="us-east-1"
        THE_BUTLER_SAYS_SO=credentials('aws-creds')
    }

    stages {
        stage('Hello') {
            steps {
                    sh '''
                        aws --version
                        aws ec2 describe-instances
                    '''

            }
        }
    }
}
