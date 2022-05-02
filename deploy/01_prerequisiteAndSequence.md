# Prerequisite & Sequence

## prerequisite
- spa project
- pipeline tool(이 예제에선 bitbucket pipeline)
- s3 bucket
- ec2
- codedeploy application & group
- two aws iam roles
 
<br/>

## sequence
1. pipeline 작성(tool에 따라 많이 다름)
2. ec2를 사용하는 경우엔 codedeploy필수, 아니라면 sftp로도 가능(sshpass 사용)
3. deploy용 s3 bucket 생성
4. codedeploy application, group 생성(생성시 iam 역할 필요 codedeploy)
5. 배포할 ec2에 iam 역할 연결(AmazonEC2RoleforAWS-CodeDeploy)
6. ec2에 codedeploy-agent 설치 -> 재부팅

<br/>

## caution
- codedeploy, s3, ec2 region