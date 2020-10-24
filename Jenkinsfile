pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'building'
      }
     post{
      always{
       jiraSendBuildInfo branch: '', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
