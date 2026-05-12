pipeline{
  agent any
  environment{
    DOCKER_IMAGE='anjaneyak97/my-python-app'
    DOCKER_TAG='latest'
  }
  stages{
    stage("build images"){
      sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG'
    }
    stage('Login with credentials'){
          withCredentials([usernamePassword(
            credentialId:'dockerhub-credentials',
            userVariable:'USER',
            passwordVariable:'PASS'
            )])
          sh 'echo $USER | docker login -u $USER --$PASS-stdin'
          }  
    stage('push the image'){
      sh'docker push $DOCKER_IMAGE:$DOCKER_TAG'
    }
    stage('deploy the image'){
      sh '''
      docker s top myapp-container || true
      docker rm myapp-container ||true
      docker run -d -p 5000:5000 --name myapp-container $DOCKER_IMAGE:DOCKER_TAG
      '''
    }
  }
}
