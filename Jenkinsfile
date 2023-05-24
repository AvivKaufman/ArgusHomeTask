node {
  stage('Build & Deploy') {
    sh """
        sudo docker rm -f my_script_container || true
        sudo docker image rm -f my_script_image || true
    """    
    deleteDir()
    checkout scmGit(
    branches: [[name: "${BRANCH}"]],
    userRemoteConfigs: [[credentialsId:  '94339b6c-d33d-4ef9-8d2a-6b131153c69c',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
    sh """
        chmod 755 -R $WORKSPACE
        sudo docker build -t my_script_image .
        sudo docker image ls
        sudo docker run -d --name my_script_container my_script_image
        sudo docker exec my_script_container ls -ltr /app
        sudo docker exec my_script_container python /app/script.py
        sudo docker cp my_script_container:/app/system_info.txt $WORKSPACE
        ls -ltR
        aws s3 cp $WORKSPACE/system_info.txt s3://avivkaufman/system_info.txt
    """
  }       
}
