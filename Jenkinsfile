node{
   def commit_id
   stage('Checkout Git') {
	  
			checkout scm
			sh "git rev-parse --short HEAD > .git/commit-id"                        
			commit_id = readFile('.git/commit-id').trim()  
   }

   
  
   stage('Docker Build & Push') {
     
			docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			def app = docker.build("mraagil/briliancloud:nginx", '.').push()
			notifyDocker()
  } 
     
   }
   
   
}
	


