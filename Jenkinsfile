properties([pipelineTriggers([githubPush()])])

pipeline {
       agent {
        label 'github-prabhat'
    }
  tools {
    maven 'Maven'
  }
  environment {
   registry = "prabhat2020/testing4"
   registryCredential = "06216ef3-ad77-49a6-a37b-e2e91cd08bfb"
  }
  stages {
         
         stage('Checkout SCM') {
            steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'master']],
                 userRemoteConfigs: [[
                    url: 'git@github.com:prabhat2020/Jira.git',
                    credentialsId: '',
                 ]]
                ])
            }
        }
         
         
         
    stage('Initialize'){
      steps{
        echo "We are doing some test for integration"
        echo "PATH = ${PATH}"
        }
    }
    stage('Build'){
           steps
           {
        sh "mvn clean install"
    }     
    }
        
  }
post {
       always {
            echo 'I will always say Hello again!'
     create_newjira_issue()
       }
    }

}
void create_newjira_issue() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'DEV'],
            summary: 'Maven Build',
            description: 'Facing some issue in building Maven Code',
            issuetype: [name:'Task']]]


    response = jiraNewIssue issue: NewJiraIssue ,site: 'JIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}



