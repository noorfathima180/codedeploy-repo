pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main',
                url: 'https://github.com/noorfathima180/codedeploy-repo.git'
            }
        }

        stage('Zip Application') {
            steps {
                sh 'rm -f codedeploy.zip'
                sh 'zip -r codedeploy.zip index.html appspec.yml'
            }
        }

        stage('Upload to S3') {
            steps {
                sh 'aws s3 cp codedeploy.zip s3://noor1-bucket/'
            }
        }

        stage('Trigger CodeDeploy') {
            steps {
                sh '''
                aws deploy create-deployment \
                --application-name noor-codedeploy \
                --deployment-group-name noor-deployment-group \
                --s3-location bucket=noor1-bucket,key=codedeploy.zip,bundleType=zip \
                --ignore-application-stop-failures \
                --region ap-south-2
                '''
            }
        }
    }
}
