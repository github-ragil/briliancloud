node{
   def commit_id
   stage('Checkout Git') {
	 try { 
			checkout scm
			sh "git rev-parse --short HEAD > .git/commit-id"                        
			commit_id = readFile('.git/commit-id').trim()
			notifyStarted()
  }  catch (e) {
		currentBuild.result = "FAILED"
		notifyFailedGit()
		throw e
  }
   }

   stage('Installing dependencies') {
     nodejs(nodeJSInstallationName: 'nodejs') {	 
	   try { 
			sh 'npm install'
			notifyInstall()
  }  catch (e) {
		currentBuild.result = "FAILED"
		notifyFailedInstall()
		throw e
  }
     }	 
   }
   
    

   stage('Testing') {
     nodejs(nodeJSInstallationName: 'nodejs') {
	 try { 
			sh 'npm test'
			notifyConnected()
  }  catch (e) {
		currentBuild.result = "FAILED"
		notifyFailedConnected()
		throw e
  }
     	 
     }
   }
  
   stage('Docker Build & Push') {
     
		try { 
			docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			def app = docker.build("mraagil/docker-nodejs:mp06", '.').push()
			notifyDocker()
			}
  }  catch (e) {
		currentBuild.result = "FAILED"
		notifyFailedDocker()
		throw e
  }
     
   }
   
   stage('Docker Pull & Deploy Scale Out') {
		try { 
			sh 'docker pull mraagil/docker-nodejs:mp06'
			sh 'ssh root@192.168.200.16 sudo bash /home/deploy.sh'
			notifyPull()
			notifySuccessful()
  }  catch (e) {
		currentBuild.result = "FAILED"
		notifyFailedPull()
		throw e
  }
		
   }
   
}
	


