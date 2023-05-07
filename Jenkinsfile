pipeline {
	agent any

	stages {

		stage("Create S3 Bucket") {
			steps{
				sh "echo creating s3 bucket..."
			}
		}

		stage("Create DynamoDB") {
			steps{
				sh "echo creating DynamoDB..."
			}
		}

		stage("Create ECR_REPO") {
			steps{
				sh "echo creating ECR_REPO..."
			}
		}

		stage("Terraform Init & Apply") {
			steps{
				sh "echo initializing and applying Terraform..."
			}
		}

		stage("AWS ECR Authentication") {
			steps{
				sh "echo AWS ECR Authentication..."
			}
		}

		stage("Deploy App Image") {
			steps{
				sh "echo Deploy App Image..."
			}
		}
		

	}

}