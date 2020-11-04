pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'Build should work...'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'master', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
