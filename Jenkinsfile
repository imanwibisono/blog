env.DOCKER_REGISTRY = 'imanwibi'
env.DOCKER_IMAGE_NAME = 'blog'
node('master') {
	stage('HelloWorld') {
      echo 'Hello World'
    }
    stage('Git Pull from Github') {
      git credentialsId: 'github_imanwibisono', brach: "master", url: 'https://github.com/imanwibisono/blog.git'
    }
      stage('Build Docker Image') {
        sh "docker build --build-arg APP_NAME=blog -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:p-${BUILD_NUMBER} ."   
    }
      stage('Push Docker Image to Dockerhub') {
          sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:p-${BUILD_NUMBER}"
    }
      stage('DeployTo Kubernetes Cluster') {
        kubernetesDeploy(
          kubeconfigId: 'kube_config',
          configs: 'config_kubernetes.yml',
          enableConfigSubstitution: true
        )
    }
    stage('Remove Docker Image') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:p-${BUILD_NUMBER}"   
    }
}