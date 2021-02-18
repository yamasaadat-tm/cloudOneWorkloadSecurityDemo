# cloudOneWorkloadSecurityDemo

These scripts will help you trigger events in Cloud One Workload Security

**Note:** These scripts are provided as-is with no implied support.  You are welcome to comment if you find issues, but there's no guarantee on if or when they'll be fixed.

Before running these scripts, ensure you have the following:
1)	A Cloud One Workload Security Account

2)	A Ubuntu 18.04, Redhat 8, Amazon Linux or Windows Server

3)	The system under test must have:
    * python3
    * python3-pip
    * git
    * curl
    * wget
    * netcat (Linux Only)
    * unzip

    `sudo yum  install -y python3 python3-pip git curl unzip wget jq`

                                    or

    `sudo apt install -y python3 python3-pip git curl unzip wget jq`

4)	You have installed the agent on the system and activated it in your Cloud One Workload Security console. For this step, you can use the code provided if you have an API Key generated from within the Cloud One console. If you don't have an API Key, refer to the Help Docs here - https://cloudone.trendmicro.com/docs/workload-security/api-cookbook-set-up/#create-an-api-key

    `export WorkloadSecurityApiKey=<Your API Key>`

    `curl -L -X POST 'https://app.deepsecurity.trendmicro.com/api/agentdeploymentscripts' `
    `-H 'Content-Type: application/json' `
    `-H 'api-version: v1' `
    `-H 'api-secret-key: '"${WorkloadSecurityApiKey}"'' `
    `--data-raw '{ `
    `"platform": "linux", `
    `"validateCertificateRequired": false, `
    `"validateDigitalSignatureRequired": false, `
    `"activationRequired": true, `
    `"policyID": 432 `
    `}' | jq --raw-output .scriptBody > ~/dsa_deploy.sh`

    `chmod +x ~/dsa_deploy.sh`

    `sudo bash ~/dsa_deploy.sh`

    `rm ~/dsa_deploy.sh # Optional Step`

5)	The files from this repository
    * Download these files and put them in a directory on the system under test

        `git clone https://github.com/GeorgeDavis-TM/cloudOneWorkloadSecurityDemo.git`

6)	The Python SDK for Deep Security/Cloud One Workload Security
    * Download the SDK from: https://automation.deepsecurity.trendmicro.com/sdk/20_0/v1/dsm-py-sdk.zip and put it in the same folder as the files you just downloaded

        `wget -P ~ https://automation.deepsecurity.trendmicro.com/sdk/20_0/v1/dsm-py-sdk.zip`

    * Unzip the file

        `unzip ~/dsm-py-sdk.zip`

    * Install the sdk dependencies from within the directory: 
        
        `sudo python3 -m pip install .`

                or

        `sudo pip3 install .`
    
7)	Add the API Key to the `config.json` file

    * Find the line `"api-secret-key" = "<Your API Key>"`

    * Change `<Your-API-Key>` to your actual API key. If you don't have an API Key, refer to the Help Docs here - https://cloudone.trendmicro.com/docs/workload-security/api-cookbook-set-up/#create-an-api-key

    * Once you've added your API Key, save the file.

                    or

        `cd cloudOneWorkloadSecurityDemo`
        
        `sed -i 's/<Your API Key>/'${WorkloadSecurityApiKey}'/g' config.json`

8) Install python script dependencies using the requirements.txt file.

    `pip3 install -r requirements.txt`

9) Configure your `config.json` file if you need to run in quiet mode.

### Config.json parameters -

| Fields | Type | Description | Required? |
|--------| ---- | ----------- | --------- |
|`tests` | List | Contains all the different tests you can run from this utility | Yes |
| `policyName` | String | The policy that is targeted for a demo attack | Yes |
| `hostName` | String | The exact hostname of the instance targeted | Yes |
| `confirmation` | Boolean | Override input confirmation to run the tests | Yes (for Quiet mode) |
| `dsmHost` | String | Cloud One Workload Security / Deep Security Manager URL (Default: `https://cloudone.trendmicro.com:443`) | Optional. Used only for non-SaaS DSM tests |
| `api-secret-key` | String | Cloud One WOrkload Security / Deep Security API Key. You can create an API Key using these instructions - https://cloudone.trendmicro.com/docs/workload-security/api-cookbook-set-up/#create-an-api-key | Yes |


Now you can run the script using `python3 cloud_one_workload_security_demo.py`


### Related Projects

| GitHub Repository Name  | Description |
| ------------- | ------------- |
| [WorkloadSecurityConnector-AWS](https://github.com/GeorgeDavis-TM/WorkloadSecurityConnector-AWS) | Automation scripts to setup the AWS Connector on Trend Micro Cloud One Workload Security / Deep Security (On-Prem on AWS) |
| [WorkloadSecurity-AWS-SNS](https://github.com/GeorgeDavis-TM/WorkloadSecurity-AWS-SNS) | Setup Event forwarding with AWS SNS to build custom rules and workflow based on detection events |