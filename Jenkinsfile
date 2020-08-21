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
       success 
        {
         create_newjira_issuesuc()
        }
        failure
       {
        create_newjira_issuefai()
       }
       }
    

}

void create_newjira_issuefai() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'DEV'],
            summary: 'Build Failed',
            description: 'Build failed! need to see code',
            issuetype: [name:'Task']]],
            assignee: 'kprabhat0123@outlook.com'


    response = jiraNewIssue issue: NewJiraIssue, site:'JIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}
void create_newjira_issuesuc() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'DEV'],
            summary: 'Build Success',
            description: 'Successfully built! Yay',
            issuetype: [name:'Task']]],
            assignee: 'kprabhat0123@outlook.com'


    response = jiraNewIssue issue: NewJiraIssue, site:'JIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}



