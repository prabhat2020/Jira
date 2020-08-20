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
         
         
         stage('Checkout SCM') {
            steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'master']],
                 userRemoteConfigs: [[
                    url: 'git@github.com:prabhat2020/Jira.git',
                    
                 ]]
                ])
            }
        }
         
         
         
    stage('Build'){
           steps
           {
        sh "mvn clean install"
    }     
    }
         stage('Sonar'){
                steps
                {
        
            sh "mvn sonar:sonar -Dsonar.projectKey=SpringPipeline -Dsonar.host.url=http://52.143.7.186/prabhat-sonarqube -Dsonar.login=0144fc0a3377b5a9fd3802be5bdf248a9b0067ae"
                }
     }
stage('Building our image') { 
             steps { 
                    script {
                 dockerImage = docker.build registry 
                    }
                 } 

     }
      stage('Deploy our image') { 
          steps { 
                 script { 
                  docker.withRegistry( '', registryCredential ) { 
                  dockerImage.push() 
                        }
                   } 
          }
      }
         
  
       stage('Deploy Application on K8s') {
              steps
              {
                sh("curl -LO https://storage.googleapis.com/kubernetes-release/release/\$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl")
                sh("chmod +x ./kubectl")
                sh("cat ./spring.yaml | ./kubectl apply -f -")
                echo "Application started on port: HTTP_PORT (http)"
    }
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


    response = jiraNewIssue issue: NewJiraIssue, site: 'http://51.145.191.144:8080'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}





