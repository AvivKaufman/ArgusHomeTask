node {
  stage('Clone') {
    checkout scmGit(
    branches: [[name: "${BRANCH}"]],
    userRemoteConfigs: [[credentialsId:  'kaufman905@gmail.com',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
  }       
}
