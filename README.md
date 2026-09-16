export STUDENT_ID="student13" 
export AWS_PROFILE="$STUDENT_ID"
export AWS_REGION="ap-northeast-2"
export AWS_PAGER=""

export MY_KEY_NAME="${STUDENT_ID}-key"
export MY_SG_NAME="${STUDENT_ID}-web-sg"
export MY_INSTANCE_NAME="${STUDENT_ID}-managed-ec2"
export MY_REUSE_INSTANCE_NAME="${STUDENT_ID}-compose-ec2"
export MY_DATA_SG_NAME="${STUDENT_ID}-data-sg"
export MY_DB_ID="${STUDENT_ID}-mysql-db"
export MY_DB_SUBNET_GROUP="${STUDENT_ID}-db-subnet-group"
export MY_CACHE_ID="${STUDENT_ID}-redis"
export MY_CACHE_SUBNET_GROUP="${STUDENT_ID}-cache-subnet-group"
export MY_AMI_NAME="${STUDENT_ID}-app-image"
export MY_INSTANCE_NAME_2="${MY_INSTANCE_NAME}-2"
export MY_TG_NAME="${STUDENT_ID}-app-tg"
export MY_ALB_SG_NAME="${STUDENT_ID}-alb-sg"
export MY_ALB_NAME="${STUDENT_ID}-app-alb"

# 로그인 필요
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
export MY_BUCKET="${STUDENT_ID}-app-assets-${ACCOUNT_ID}"
export MY_APP_IMAGE="ghcr.io/a1l1ke/simple-back-ghcr:latest"
export ACTIVE_INSTANCE_NAME="$MY_INSTANCE_NAME"

aws sts get-caller-identity
aws configure sso --profile "${STUDENT_ID}"
aws sso login --profile "${STUDENT_ID}"


# 기존 키 삭제 및 신규 발급
aws ec2 delete-key-pair --key-name "$MY_KEY_NAME"
aws ec2 create-key-pair --key-name "$MY_KEY_NAME" \
--tag-specifications "ResourceType=key-pair,Tags=[{Key=Name,Value=$MY_KEY_NAME},{Key=Course,Value=infra-training},{Key=Owner,Value=$STUDENT_ID}]" \
--query "KeyMaterial" --output text > ./"$MY_KEY_NAME".pem
chmod 400 ./"$MY_KEY_NAME".pem