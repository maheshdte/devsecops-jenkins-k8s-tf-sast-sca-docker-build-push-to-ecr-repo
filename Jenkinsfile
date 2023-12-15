pipeline {
  agent any
  tools { 
        maven 'maven'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=devsecops-dec-prac -Dsonar.organization=devsecops-dec-prac -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=e5c2939104651c4d74f2e1d9dd8c4dc2c440cccb'
			}
    }

    stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }		
  }

    stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("maheshdte")
                 }
               }
            }
    }

    stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://771505854103.dkr.ecr.us-east-1.amazonaws.com/maheshdte', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}

