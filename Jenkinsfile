pipeline{
   def commit_id
   stage('Checkout Git') {
	 try { 
			checkout scm
			sh "git rev-parse --short HEAD > .git/commit-id"                        
			commit_id = readFile('.git/commit-id').trim()
	}  
   }

   
  
   stage('Docker Build & Push') {
     
		try { 
			docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			def app = docker.build("mraagil/briliancloud:nginx", '.').push()
			notifyDocker()
			}
  } 
     
   }
   
   
}
	


