node {
  if(env.BUILD_DEPLOY_STAGE.toBoolean()) {
    def timerCause = currentBuild.rawBuild.getCause(hudson.triggers.TimerTrigger.TimerTriggerCause)
    if (timerCause) {
        println "Build reason was started by timer, will not run the stage"
    } else {
        stage('Build & Deploy') {
            sh """
                sudo docker rm -f my_script_container || true
                sudo docker image rm -f avivkaufman || true
            """    
            deleteDir()
            checkout scmGit(
            branches: [[name: "${BRANCH}"]],
            userRemoteConfigs: [[credentialsId:  '94339b6c-d33d-4ef9-8d2a-6b131153c69c',
                url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
            sh """
                chmod 755 -R $WORKSPACE
                sudo docker build -t avivkaufman .
                sudo docker run -d --name my_script_container avivkaufman
                sudo docker exec my_script_container python /app/script.py
                sudo docker cp my_script_container:/app/system_info.txt $WORKSPACE
            """
            withAWS(credentials: '10dcb875-ccbe-4bde-93ab-b43a425a70c8', region: "us-east-1") {
                s3Upload acl: 'Private', bucket: 'avivkaufman', file: 'system_info.txt'
                sh "sudo docker login -u AWS -p \$(aws ecr get-login-password --region us-east-1) 161192472568.dkr.ecr.us-east-1.amazonaws.com"
                sh "sudo docker tag avivkaufman:latest 161192472568.dkr.ecr.us-east-1.amazonaws.com/avivkaufman:latest"
                sh "sudo docker push 161192472568.dkr.ecr.us-east-1.amazonaws.com/avivkaufman:latest"
            }
        }
    }
  }
  if(env.ghprbPullId) {
    println "Not triggeting pull stage because its a PR"
    } else {
        if(env.PULL_TEST_STAGE.toBoolean()) {
          stage('Pull & Test') {
              withAWS(credentials: '10dcb875-ccbe-4bde-93ab-b43a425a70c8', region: "us-east-1") {
                  s3Download(file: 'system_info_from_bucket.txt', bucket: 'avivkaufman', path: 'system_info.txt', force: true)
              }
              sh "cat system_info_from_bucket.txt"
          }
        }    
    }


}
