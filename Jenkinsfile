String branchName = env.BRANCH_NAME
String gitCredentials = "CREDENTIAL_ID"
String repoUrl = "https://github.com/username/repo-name.git"

node {
  stage('Clone') {
    checkout scmGit(
    branches: [[name: "${BRANCH}"]],
    userRemoteConfigs: [[credentialsId:  'kaufman905@gmail.com',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
  }       
}
