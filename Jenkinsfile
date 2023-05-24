def clean_docker() {
    sh """
        docker images && docker ps -a
        docker system prune -a -f --volumes
        docker rm -f \$(docker ps -aq)
        docker images && docker ps -a
    """
node {
  stage('Build & Deploy') {
    deleteDir()
    clean_docker()
    checkout scmGit(
    branches: [[name: "${BRANCH}"]],
    userRemoteConfigs: [[credentialsId:  '94339b6c-d33d-4ef9-8d2a-6b131153c69c',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
    sh """
        chmod +x $WORKSPACE
        sudo docker build -t my_script_image .
        sudo docker image ls
        sudo docker run -d --name my_script_container -v $WORKSPACE:/app my_script_image
        ls -ltR
    """
  }       
}
