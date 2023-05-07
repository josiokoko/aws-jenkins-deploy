pipeline {
	agent any

	environment {
		AWS_DEFAULT_REGION='us-east-1'
		AWS_CREDENTIALS = credentials('aws-auth')
	}

	stages {

		stage("Create S3 Bucket") {
			steps{
				script{
					createS3Bucket('raycoy-aws-deploy-jenkins')
				}
			}
		}

		stage("Create DynamoDB") {
			steps{
				script{
					createDynamoDB('raycoy-aws-deploy-jenkins-lock')
				}
			}
		}

		stage("Create ECR_REPO") {
			steps{
				script {
					createECR('aws-deploy-cicd-repo')
				}
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



def createS3Bucket(bucketName){
    sh returnStatus: true, script: "aws s3 mb s3://${bucketName} --region=us-east-1"
}

def createECR(repoName){
    sh returnStatus: true, script: "aws ecr create-repository --repository-name ${repoName} --image-scanning-configuration scanOnPush=true --region ${AWS_DEFAULT_REGION}"
}


def createDynamoDB(dynamodbName){
    sh returnStatus: true, script: "aws dynamodb create-table --table-name ${dynamodbName} --attribute-definitions AttributeName=LockID,AttributeType=S --key-schema AttributeName=LockID,KeyType=HASH --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5"
}