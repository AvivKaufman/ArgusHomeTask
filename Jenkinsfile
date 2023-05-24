node {
  stage('Build & Deploy') {
    deleteDir()
    sh """
        sudo docker rm -f my_script_container || true
        sudo docker image rm -f my_script_image || true
    """
    checkout scmGit(
    branches: [[name: "${BRANCH}"]],
    userRemoteConfigs: [[credentialsId:  '94339b6c-d33d-4ef9-8d2a-6b131153c69c',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
    sh """
        chmod +x $WORKSPACE
        sudo docker build -t my_script_image .
        sudo docker image ls
        sudo docker run --name my_script_container -v $WORKSPACE:/app my_script_image
        ls -ltR
    """
  }       
}
