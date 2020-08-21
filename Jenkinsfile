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
         create_jira_success()
        }
        failure
       {
        create_jira_fail()
       }
       }
    

}

void create_jira_fail() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'DEV'],
            summary: 'Build Failed',
            description: 'Build failed! need to see code',
            issuetype: [name:'Task']]]
            

    response = jiraNewIssue issue: NewJiraIssue, site:'JIRA' 

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}
void create_jira_success() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'DEV'],
            summary: 'Build Success',
            description: 'Successfully built! Yay',
            issuetype: [name:'Task']]]
            


    response = jiraNewIssue issue: NewJiraIssue, site:'JIRA' 

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}



