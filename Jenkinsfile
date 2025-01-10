pipeline {    
    agent any 

    environment {
        registry = "vishalgajam/myimage" 
        registryCredential = 'dockertoken'
        '''mavenHome  = tool 'myMaven'
	PATH = "$mavenHome/bin:$PATH"'''
        
    }
   
    stages {   
       stage('Checkout') {
			steps {
				'''echo "$PATH"'''
				echo "$env.BUILD_NUMBER"
				echo "$env.BUILD_ID"
				echo "$env.JOB_NAME"
				echo "$env.BUILD_TAG"
				echo "$env.BUILD_URL"
				'''sh "mvn --version"'''
			}
		}
       stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":V$BUILD_NUMBER" 
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
     stage('List Docker Images after Build') {
            steps {
                script {
                    sh 'docker images'
                }
            }
        }
      stage('Remove Images after push') {
           steps {
                script{
                    sh "docker rmi $registry:V$BUILD_NUMBER" 
                }    
            }
        } 
    }
}
