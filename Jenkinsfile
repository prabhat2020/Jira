properties([pipelineTriggers([githubPush()])])

pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  
  stages {
         
         stage('Checkout SCM') {
            steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'master']],
                 userRemoteConfigs: [[
                    url: 'https://github.com/prabhat2020/Jira.git',
                    credentialsId: 'github-prabhat',
                 ]]
                ])
            }
        }
         
         
         
    stage('Initialize'){
      steps{
        echo "test for pipeline"
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
            echo 'check for jira'
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



