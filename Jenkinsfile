 node{
         stage('SCM Checkout'){
         git 'https://github.com/manickavelsivakumar/my-app.git'
         }
         stage('Compile-Package'){
	         def mvnHome =  tool name: 'maven3', type: 'maven'   
	         sh "${mvnHome}/bin/mvn clean package"
	         sh 'mv target/myweb*.war target/newapp.war'
         }
         stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
        }
        }
         stage('Build Docker Imager'){
         sh 'docker build -t manickavelsivakumar/myweb:0.0.2 .'
        }
         stage('Docker Image Push'){
         withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]){                       
         sh "docker login -u manickavelsivakumar -p ${dockerPassword}"
        }
         sh 'docker push manickavelsivakumar/myweb:0.0.2'
        }
         stage('Nexus Image Push'){
         sh "docker login -u admin -p 123 3.108.52.50:8083"
         sh "docker tag manickavelsivakumar/myweb:0.0.2 3.108.52.50:8083/doc:1.0.0"
         sh 'docker push 3.108.52.50:8083/doc:1.0.0'
        }
         stage('Remove Previous Container'){
     	try{
		sh 'docker rm -f tomcattest2'
     	}catch(error){
		//  do nothing if there is an exception
     	}
         stage('Docker deployment'){
         sh 'docker run -d -p 8090:8080 --name tomcattest2 manickavelsivakumar/myweb:0.0.2' 
        }
        }
        }
