node {
  stage('Build & Deploy') {
    checkout scmGit(
    branches: [[name: "${BRANCH}"]],
    userRemoteConfigs: [[credentialsId:  '94339b6c-d33d-4ef9-8d2a-6b131153c69c',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
    sh """
        sudo docker build -t my_script_image .
        sudo docker image ls
        sudo docker run -d --name my_script_container -v $WORKSPACE:/app my_script_image
        ls -ltR
    """
  }       
}
