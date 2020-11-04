pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'building is successful...'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'master', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
