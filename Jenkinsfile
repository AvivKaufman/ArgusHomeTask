node {
  stage('Clone') {
    checkout scmGit(
    branches: [[name: "Testing"]],
    userRemoteConfigs: [[credentialsId:  'kaufman905@gmail.com',
        url: 'git@github.com:AvivKaufman/ArgusHomeTask.git']]) 
  }       
}
