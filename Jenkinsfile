pipeline {
  agent any 
  
      stages {
	  
	  stage('Cloning Git') {
	  steps{
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
	   }
    } 
      stage ('Check-Git-Secrets') {
   steps {
    sh 'rm trufflehog || true'
    sh 'docker run gesellix/trufflehog --json https://github.com/sindhuhack/Devsecops.git > trufflehog'
    sh 'cat trufflehog'
    }
}
      stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/sindhuhack/Devsecops/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
      

    stage('Build-and-Tag') {

       app = docker.build("sindhuhack/snake")
    }
    stage('Post-to-dockerhub') {
        sh 'echo Post-to-dockerhub'
    
     docker.withRegistry('https://registry.hub.docker.com', 'docker_cred') {
            app.push("latest")
        			} 
         }
    
  
    
    stage('Pull-image-server') {
        sh 'echo Pull-image-server'
    
         sh "docker-compose down"
         sh "docker-compose up -d" 	
      }

   }
}
