pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'building'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'BuildInfo', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
