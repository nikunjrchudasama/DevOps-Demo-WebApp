pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'go now.. is successful...'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'master', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
