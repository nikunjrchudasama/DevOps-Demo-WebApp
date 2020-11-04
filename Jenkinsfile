pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'building test..'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'master', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
