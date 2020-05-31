pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building docker image'
        sh '''# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${GIT_COMMIT}-${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
'''
        echo 'Build complete'
      }
    }

    stage('Push to registry') {
      steps {
        echo 'Pushing image to ACR'
        sh '''WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${GIT_COMMIT}-${BUILD_NUMBER}"
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME'''
        echo 'Image push successful'
      }
    }

    stage('Deploy to AKS') {
      steps {
        echo 'Deploying to AKS cluster'
        sh '''WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${GIT_COMMIT}-${BUILD_NUMBER}"
/var/jenkins_home/kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/jenkins_home/config'''
      }
    }

  }
  environment {
    ACR_LOGINSERVER = 'votingjrs.azurecr.io'
  }
}