pipeline {
    agent any
    
    tools{
        maven 'Maven3.6.3' 
    }
    
    environment {
        SONAR_ADMIN_CRED = credentials('sonar-admin')
    }

    stages {
        
        stage('SourceCode-Checkhout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/nikunjrchudasama/DevOps-Demo-WebApp/']]])
            }
        }
        
        stage('Code-Analysis'){
            steps{
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn $SONAR_MAVEN_GOAL -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=$SONAR_ADMIN_CRED_USR -Dsonar.password=$SONAR_ADMIN_CRED_PSW sonar:sonar -f pom.xml'
                }
            }
        }
        
        stage('Build'){
            steps{
                sh 'mvn clean install -f pom.xml'
            }
            post{
                always{
                    jiraSendBuildInfo branch: 'CAS-1', site: 'nikunjrchudasama.atlassian.net'
                }
            }
        }
        
        stage('Upload-Artifact'){
            steps{
                
                rtUpload (
                    serverId: 'artifactory-1',
                    spec: """{
                            "files": [
                                    {
                                        "pattern": "target/*.war",
                                        "target": "libs-release-local"
                                    }
                                ]
                            }"""
                )
                
                rtPublishBuildInfo (
                    serverId: 'artifactory-1'
                )
            }
        }
        
        stage('Deploy-to-QA'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://104.40.61.43:8080')], contextPath: '/QAWebapp', onFailure: false, war: '**/*.war'
            }
            post{
                always{
                     jiraSendDeploymentInfo environmentId: 'us-test-1', environmentName: 'us-test-1', environmentType: 'testing', serviceIds: [''], site: 'nikunjrchudasama.atlassian.net', state: 'successful'
                }
            }
        }
        
        stage('Slack-Notification'){
            steps{
                slackSend channel: 'alerts', message: "Deployed to QA: Job - '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
            }
        }
       
        stage('UI-Test'){
            steps{
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'UI Test Report', reportTitles: ''])
            }
        }
        
        stage('Performance-Test'){
            steps{
                blazeMeterTest credentialsId: 'Blazemeter', testId: '8659547.taurus', workspaceId: '683506'
            }
        }
        
        stage('Deploy-to-PROD'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://104.40.61.26:8080/')], contextPath: '/ProdWebapp', onFailure: false, war: '**/*.war'
            }
            post{
                always{
                     jiraSendDeploymentInfo environmentId: 'us-prod-1', environmentName: 'us-prod-1', environmentType: 'production', serviceIds: [''], site: 'nikunjrchudasama.atlassian.net', state: 'successful'
                }
            }
        }
        
        stage('Slack-Notification-Prod'){
            steps{
                slackSend channel: 'alerts', message: "Deployed to PROD: Job - '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
            }
        }
        
        stage('Sanity-Test'){
            steps{
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\Acceptancetest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'SanityTestReport', reportTitles: ''])
            }
        }
    }
}
