# serverless-wsgi-django-demo

## The basics

See `/payloads` for some example payloads

### Example installation/deploy

Information about testing environment

```
spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ node -v
v14.19.2
spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ npm -v
6.14.17
```

Install the contents of package.json and use serverless and other deps at a specific version

```
spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ npm install
npm WARN serverless_wsgi_django@1.0.0 No description
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@2.3.2 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

audited 416 packages in 2.742s

56 packages are looking for funding
  run `npm fund` for details
```

### Example local development/invoke

Install updated package tooling

```
spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ python3.8 -m venv .venv
spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ source .venv/bin/activate
(.venv) spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ pip install --upgrade pip setuptools wheel pip-tools
...
Successfully installed click-8.1.3 pep517-0.12.0 pip-22.1.2 pip-tools-6.6.2 setuptools-62.3.2 tomli-2.0.1 wheel-0.37.1
```

Install requirements/requirements-dev

```
(.venv) spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ pip install -r ./requirements.txt -r ./requirements-dev.txt
...
Installing collected packages: mypy-extensions, mccabe, werkzeug, urllib3, typing-extensions, sqlparse, six, pyflakes, pycodestyle, platformdirs, pathspec, jmespath, django-types, backports.zoneinfo, asgiref, python-dateutil, mypy-boto3-sqs, mypy-boto3-s3, mypy-boto3-rds, mypy-boto3-lambda, mypy-boto3-ec2, mypy-boto3-dynamodb, mypy-boto3-cloudformation, mypy, flake8, django, botocore-stubs, black, botocore, boto3-stubs, s3transfer, boto3
Successfully installed asgiref-3.5.2 backports.zoneinfo-0.2.1 black-22.3.0 boto3-1.24.0 boto3-stubs-1.24.0 botocore-1.27.0 botocore-stubs-1.27.0 django-4.0.4 django-types-0.15.0 flake8-4.0.1 jmespath-1.0.0 mccabe-0.6.1 mypy-0.960 mypy-boto3-cloudformation-1.24.0 mypy-boto3-dynamodb-1.24.0 mypy-boto3-ec2-1.24.0 mypy-boto3-lambda-1.24.0 mypy-boto3-rds-1.24.0 mypy-boto3-s3-1.24.0 mypy-boto3-sqs-1.24.0 mypy-extensions-0.4.3 pathspec-0.9.0 platformdirs-2.5.2 pycodestyle-2.8.0 pyflakes-2.4.0 python-dateutil-2.8.2 s3transfer-0.6.0 six-1.16.0 sqlparse-0.4.2 typing-extensions-4.2.0 urllib3-1.26.9 werkzeug-2.1.2
```

Use `npm run serverless` to do a local test

```
(.venv) spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ npm run serverless -- invoke local --function api --path payloads/helloworld.request.json

> serverless_wsgi_django@1.0.0 serverless /home/spencersr/src/serverless/serverless-wsgi-django-demo
> serverless "invoke" "local" "--function" "api" "--path" "payloads/helloworld.request.json"

{
    "statusCode": 200,
    "headers": {
        "Content-Type": "text/html; charset=utf-8",
        "X-Frame-Options": "DENY",
        "Content-Length": "13",
        "X-Content-Type-Options": "nosniff",
        "Referrer-Policy": "same-origin",
        "Cross-Origin-Opener-Policy": "same-origin"
    },
    "body": "Hello, World!",
    "isBase64Encoded": false
}
```

Now with docker

```
(.venv) spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ npm run serverless -- invoke local --function api --path payloads/helloworld.request.json --docker

> serverless_wsgi_django@1.0.0 serverless /home/spencersr/src/serverless/serverless-wsgi-django-demo
> serverless "invoke" "local" "--function" "api" "--path" "payloads/helloworld.request.json" "--docker"

Using Python specified in "runtime": python3.8
Packaging Python WSGI handler...
START RequestId: 3acf7c9b-baf9-1e00-07ca-0f1962bdff67 Version: $LATEST
END RequestId: 3acf7c9b-baf9-1e00-07ca-0f1962bdff67
REPORT RequestId: 3acf7c9b-baf9-1e00-07ca-0f1962bdff67  Init Duration: 1007.15 ms       Duration: 21.52 ms      Billed Duration: 22 ms  Memory Size: 1024 MB    Max Memory Used: 46 MB

{"statusCode":200,"headers":{"Content-Type":"text/html; charset=utf-8","X-Frame-Options":"DENY","Content-Length":"13","X-Content-Type-Options":"nosniff","Referrer-Policy":"same-origin","Cross-Origin-Opener-Policy":"same-origin"},"body":"Hello, World!","isBase64Encoded":false}
```

### Example Deploy/Fetch

Use `npm run serverless` to optionally deploy

```
(.venv) spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ npm run serverless -- deploy --stage test

> serverless_wsgi_django@1.0.0 serverless /home/spencersr/src/serverless/serverless-wsgi-django-demo
> serverless "deploy" "--stage" "test"


Deploying serverless-wsgi-demo to stage test (us-east-1)
Using Python specified in "runtime": python3.8
Packaging Python WSGI handler...

✔ Service deployed to stack serverless-wsgi-demo-test (74s)

endpoint: ANY - https://shwx61emn9.execute-api.us-east-1.amazonaws.com
functions:
  api: serverless-wsgi-demo-test-api (21 MB)
```

```
(.venv) spencersr@gtd:~/src/serverless/serverless-wsgi-django-demo$ curl https://shwx61emn9.execute-api.us-east-1.amazonaws.com
Hello, World!
```
