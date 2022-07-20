pipeline 
{
   agent any
    tools{
        maven '3.5.4'
    }
    stages{
        stage('Build Maven')
		{
            steps
			{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/salmanspice/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }

    stage('Build docker image')
		{
            steps
			{
                script
				{
                    sh 'docker build -t 9958466228/pipeline .'
                }
            }
        }
		stage('Push image to Hub')
		{
            steps
			{
                script
				{
                   withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) 
				   {
                   sh 'docker login -u 9958466228 -p ${dockerhubpwd}'

                   }
                   sh 'docker push 9958466228/pipeline'
                }
            }
       }
	   stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
	}   
}   
