pipeline{
 agent any
  stages{
    stage('build'){
      steps{
        echo 'Demo build for entire pipeline!!..'
      }
     post{
      always{
       jiraSendBuildInfo branch: 'master', site: 'nikunjrchudasama.atlassian.net'
      }
     }
    }  
  }
}
