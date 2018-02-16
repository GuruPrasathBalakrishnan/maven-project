pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
		stage('Deploy to staging'){
			steps{
				build job: 'deploy-to-staging'
			}
		}
		stage('Deploy to Prod'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message:'Approve DEPLOYMENT to production?'
				}
				build job: 'deploy-to-prod'
			}
			post{
				success{
					echo 'Code deployed to production'
				}
				failure{
					echo 'Code not deployed to production'
				}
			}
		}
    }
}
