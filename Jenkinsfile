
//declarative pipeline

pipeline  
{

	agent any
        environment
		{
			dockerhome= tool 'myDocker'
			mavenHome= tool 'myMaven'
			PATH= "$dockerHome/bin:$mavenHome/bin:PATH"
		}

		stages
		{

			stage ('checkout') 
			{
            steps{
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "BUILD_ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD_TAG - $env.BUILD_TAG"
			    echo "BUILD_URL - $env.BUILD_URL"
				 }
			}
			stage ('compile')
			{
 				steps{
					sh 'mvn clean compile'
				}
			}

			
             stage('Build')
		    {
		        steps {
                      echo "Build"
                       					
				}
			 }
			stage('Test')	
			{ steps {
                      
                       sh "mvn test"
					   				
				}
			}
				
			stage('IntegrationTest')
			{steps {
                      
					   //echo "IntegrationTest"					
				   sh "mvn failsafe:integration-test failsafe:verify"
				}
		    }
			stage('SkipIntegrationTest')
			{steps {
                      
					   //echo "IntegrationTest"					
				   sh "mvn package -DskipTests"
				}
		    }

            stage ('Build Docker Image')
            {
	         steps{
		//docker build -t ankitbhatt2230/my-first-docker-repo:$env.BUILD_TAG
		     script{
			      docker.build("ankitbhatt2230/my-first-docker-repo.${env.BUILD_TAG}")
		           }

	              }
            }     
           stage('docker push image')
           {
	         steps{
		       script{
				docker.withRegistry("",'dockerhub')			   
		        dockerImage.push();
		        dockerImage.push('latest')
		        }
		
	             }
		   }
		}
}


