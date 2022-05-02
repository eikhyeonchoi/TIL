# pipeline image example(Bitbucket)
- htps://bitbucket-pipelines.atlassian.io/validator << validate 해야함
- pipeline작성 

```
image: node:16

pipelines:
  default: # default 값 (따로 branch를 설정하고 싶지 않을 경우)
    - parallel: # paralllel 설정하면 step을 동시에 진행한다 
      - step:
          name: # step name 
          script:
            # write your script
  branches: # 브랜치 설정
    develop: # branch name
      - step:
          name: dev build
          caches:
            - node
          script:
            # write your script
    master:
      - step:
          name:
          caches:
            - node
          script:
            # write your script
          artifacts:
            # 공유할 파일 및 폴더 지정할 수 있다
      - step:
          name: upload to s3
          script:
            - pipe: atlassian/aws-code-deploy:1.1.1
              variables:
                AWS_ACCESS_KEY_ID: # aws access key
                AWS_SECRET_ACCESS_KEY: # aws secret key
                AWS_DEFAULT_REGION: # aws region
                COMMAND: 'upload'
                APPLICATION_NAME: # aws codedeploy application name
                S3_BUCKET: # s3 bucket name
                ZIP_FILE: # 업로드할 zip 파일 artifacts에 지정해줘야 한다
                VERSION_LABEL: # s3에 업로드할 zip 파일 이름 
      - step:
          name: deploy to ec2
          script:
            - pipe: atlassian/aws-code-deploy:1.1.1
              variables:
                AWS_ACCESS_KEY_ID: # aws access key
                AWS_SECRET_ACCESS_KEY: # aws secret key
                AWS_DEFAULT_REGION: # aws region
                COMMAND: 'deploy'
                APPLICATION_NAME: # aws codedeploy application name
                DEPLOYMENT_GROUP: # aws codedeploy application group name
                S3_BUCKET: # 배포할 zip이 있는 s3 bucket name
                VERSION_LABEL: # 배포할 zip 파일 이름
                IGNORE_APPLICATION_STOP_FAILURES: 'true' 
                WAIT: 'true'
                FILE_EXISTS_BEHAVIOR: 'OVERWRITE'
```