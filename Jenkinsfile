node {
   def commit_id
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install'
       sh 'npm audit fix' 
     }
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io', 'dockerhub') {
       def app = docker.build("patatjaeri/nodejs_test_app:${commit_id}", '.').push()
     }
   }
}
