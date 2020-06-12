Instalation of SAM
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew --version
brew tap aws/tap
brew install aws-sam-cli
sam --version

brew upgrade aws-sam-cli

sam build
sam local invoke
sam local invoke HelloWorldFunction --event hello-world/events/event.json

sam local start-api
reminder-app$ curl http://localhost:3000/

http://localhost:3000/hello

s3_deploy_bucket="serverless-reminder-app-bucket"
stack_name_backend="serverless-reminder-backend"

aws s3 mb s3://$s3_deploy_bucket
sam package --output-template-file packaged.yml --s3-bucket $s3_deploy_bucket
sam deploy --template-file packaged.yml --stack-name $stack_name_backend --capabilities CAPABILITY_IAM
aws cloudformation delete-stack --stack-name $stack_name_backend

aws cloudformation describe-stacks --stack-name $stack_name_backend
aws cloudformation describe-stacks --stack-name $stack_name_backend --query "Stacks[0].Outputs[?OutputKey=='HelloWorldApi'].OutputValue" --output text


AWS_REGION=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/\(.*\)[a-z]/\1/')
