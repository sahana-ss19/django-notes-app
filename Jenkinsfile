pipeline{
	    agent any
	    stages{
	        stage("CLONE"){
	            steps{
	                echo " code clone"
	                git url : "https://github.com/sahana-ss19/django-notes-app.git", branch: "main"
	                echo "code clone successful"
	            }
	        }
	        stage("BUILD"){
	            steps{
	                echo "building the code"
	                sh "docker build -t notes-app:latest ."
	                echo "docker image build successfull"
	            }
	        }
	        stage("PUSH"){
	            steps{
	                echo "pushing the code"
	                   withCredentials([usernamePassword(
	                    credentialsId: 'dockerHub',
	                    usernameVariable: 'DockerHubUser',
	                    passwordVariable: 'DockerHubPass'
	                )]) {
	                    sh """
	                    echo ${DockerHubPass} | docker login -u ${DockerHubUser} --password-stdin
	                    docker tag notes-app:latest ${DockerHubUser}/notes-app:latest
	                    docker push ${DockerHubUser}/notes-app:latest
	                    """
	                }
	            }
	        }
	        stage("DEPLOY"){
	            steps{
	                echo "deploying the code"
	                sh "docker run -d -p 8000:8000 --name note-app-cont notes-app:latest"
	                echo "deployment successfull"
	            }
	        }
	    }
	}
