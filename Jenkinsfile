pipeline {
   environment {
       registry = "deegbo/jenkins-docker-test"
       DOCKER_PWD = credentials('Ye3ehGehPlu#')
   }
   agent {
       docker {
           image 'deegbo/node-docker'
	   args '-p 3000:3000'
	   args '-w /app'
	   args '-v /var/run/docker.sock:/var/run/docker.sock'
       }
   }
   options {
       skipStagesAfterUnstable()
   }
   stages {
       stage("Build"){
           steps {
               sh 'npm install'
           }
       }
       stage("Test"){
           steps {
               sh 'npm test'
           }
       }
       stage("Build & Push Docker image") {
           steps {
               sh 'docker image build -t $registry:$BUILD_NUMBER
	       sh 'docker login -u deegbo -p $DOCKER_PWD'
	       sh 'docker image push $registry:$BUILD_NUMBER'
	       sh "docker image rm $registry:$BUILD_NUMBER"
           }
       }
   }
}

