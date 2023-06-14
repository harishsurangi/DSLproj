ipipeline {
  agent any
  
  environment {
    AWS_REGION = 'ap-south-1'
    AWS_ACCOUNT_ID = '949771577427'
    ECR_REPOSITORY = 'public.ecr.aws/t5v1f4l8/dslproj'
    GITHUB_REPO_URL = 'https://github.com/harishsurangi/DSLproj.git'
    DOCKERFILE_PATH = 'https://github.com/harishsurangi/DSLproj/blob/main/Dockerfile'
    IMAGE_TAG = 'ning'
  }
  
  stages {
    stage('Clone Repository') {
      steps {
        git branch: 'main', url: "${GITHUB_REPO_URL}"
      }
    }
    
    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}", "-f ${DOCKERFILE_PATH} .")
        }
      }
    }
    
    stage('Push to ECR') {
      steps {
        withAWS(region: "${AWS_REGION}", credentials: 'your-aws-credentials-id') {
          script {
            // Authenticate Docker with ECR
            sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
            
            // Push the Docker image to ECR
            sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}"
          }
        }
      }
    }
  }
}

