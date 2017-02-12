## Deploying this function


**Requirements** Install the following packages in a new virtual environment:

    $ pip install zappa
    $ pip install flask
    $ pip install requests['secure']
    $ pip install setuptools --upgrade


Next, from within the root of the application's directory, run:  

    $ zappa init

This will take you through a simple prompt. When asked for the name of your function, make sure the path and name are correct. At the end of the prompts, you will receive a **zappa_settings.json** file. Update that file to match the following, noting that these values will be different depending on your configuration:

```javascript
{
    "develop": {
        "app_function": "healthcheck.app", 
        "s3_bucket": "your-lambda-bucket",
	"remote_env": "s3://your-lambda-bucket/auth-config.json",
        "vpc_config": {
          "SubnetIds": [
            "subnet-12345678",
            "subnet-23456789"
           ],
          "SecurityGroupIds": [
            "sg-12345678"
            ]
            },

	"project_name":"healthapi",
	"profile_name":"profile"
    }
}

```
Finally, you would update your remote_env (remote environment variables) file to include whatever environment variables you needed for your lambda function. With Zappa, the environment variables passed to a function must be stored in a simple JSON file that contains JSON object with a collection of key-value pairs; each key-value pair representing an environment variable to be exported to the system during Lambda's runtime. Zappa uses this method so that environment variables do not have to be stored in its configuration file. Instead, those secrets are stored in S3, and we use the **remote_env** key to point the file on S3 that contains these values:

```javascript
"remote_env": "s3://your-lambda-bucket/auth-config.json"
```

At this point, you'll be ready to deploy. Run zappa deploy environment_name to run your deployment. See https://github.com/Miserlou/Zappa for any issues. 

#### Important

Note that this function was written for a specific use case. In the Lambda's Python code, we are querying a resource over a specific port with a REST API running on a virtual machine, and we are parsing for specific values in the JSON response that we get from the server to determine if it's healthy or not. Obviously, based on the particular service that you are working, your code may need to differ. The advantage of this example is how using Flask (which makes handling HTTP requests and implementing simple APIs easy) along with a combination of AWS web services allows us to quickly build complex serverless architectures that are highly reliable, scalable, and extremely cost effective.
