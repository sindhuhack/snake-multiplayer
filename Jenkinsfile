node ('ubuntu-app-agent'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
     
    /*stage('SAST'){
        build 'SECURITY-SAST-SNYK'
    }*/
   
    stage ('Check-Git-Secrets') {
            
    sh 'rm trufflehog || true' 
    sh 'docker run gesellix/trufflehog --json https://github.com/sindhuhack/snake-multiplayer.git > trufflehog'
    sh 'cat trufflehog'
    }
    
    stage('Build-and-Tag') {
       // sh 'echo Build-and-Tag'
    /* This builds the actual image; synonymous to
         * docker build on the command line */
       app = docker.build("sindhuhack/snake")
    }
    stage('Post-to-dockerhub') {
        sh 'echo Post-to-dockerhub'
    
     docker.withRegistry('https://registry.hub.docker.com', 'docker_cred') {
            app.push("latest")
        			} 
         }
    /*stage('SECURITY-IMAGE-SCANNER'){
       build 'SECURITY-IMAGE-SCANNER-AQUAMICROSCANNER'
    }*/
  
    
    stage('Pull-image-server') {
        sh 'echo Pull-image-server'
    
         sh "docker-compose down"
         sh "docker-compose up -d" 	
      }
    
    /*stage('DAST')
        {
        build 'SECURITY-DAST-OWASP_ZAP'
        }*/
 
}

