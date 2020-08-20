
pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  environment {
   registry = "prabhat2020/testing4"
   registryCredential = "06216ef3-ad77-49a6-a37b-e2e91cd08bfb"
  }
  stages {
    stage('Initialize'){
      steps{
        echo "We are doing some test"
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
            issuetype: [id: '3']]]


    response = jiraNewIssue issue: NewJiraIssue ,site: 'http://51.145.191.144:8080'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}



