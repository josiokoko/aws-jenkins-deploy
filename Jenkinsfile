pipeline {
	agent any

	environment {
		PATH = "${PATH}:${getTerraformPath()}"
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
				dir('terraform/ecr_registry') {
                    sh "pwd"
                    // sh returnStatus: true, script: 'terraform workspace new dev'
                    sh "terraform init -upgrade"
                    sh "terraform apply -auto-approve -var-file=dev.tfvars"
                    script{
                        registry_id = sh(returnStdout: true, script: "terraform output registry_id").trim()
                        repository_name = sh(returnStdout: true, script: "terraform output repository_name").trim()
                        repository_url = sh(returnStdout: true, script: "terraform output repository_url").trim()
                    }
                }
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


def getTerraformPath(){
	def tfHome = tool name: 'terraform:1.4.6', type: 'terraform'
	return tfHome
}