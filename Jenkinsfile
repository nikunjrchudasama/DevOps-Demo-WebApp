pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'building'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'master', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
