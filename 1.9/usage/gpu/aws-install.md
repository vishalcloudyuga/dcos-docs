---
post_title: Configure Your AWS Cluster
nav_title: Configure Your AWS Cloud Cluster
feature_maturity: experimental
menu_order: 2
---

#  Prerequisites
- An AWS S3 bucket.
- The [AWS CLI](https://aws.amazon.com/cli/) installed.

## Generate a Custom AWS CF template for GPU

1. Download the [dcos_generate_config.sh](https://dcos.io/releases/) script to your bootstrap node.
    ```bash
    curl -O <installer>.sh
    ```
    
1. Create a directory named `genconf` in the home directory of your bootstrap node and navigate to it.
    ```bash
    $ mkdir -p genconf
    $ cd genconf
    ```

1. Create a configuration file in the `genconf` directory and save as `config.yaml`. This configuration file specifies your AWS credentials, the S3 location to store the generated artifacts, and enables GPU support on your cluster. These are the required parameters:
    ```yaml
    enable_gpu_isolation: true
    aws_template_storage_bucket: <s3-bucket-name>
    aws_template_storage_bucket_path: templates/dcos
    aws_template_upload: true
    aws_template_storage_access_key_id: <your-access-key-id>
    aws_template_storage_secret_access_key: <your-secret-access_key>
    ```

1. Run the DC/OS installer script with the AWS argument specified. This command creates and uploads a custom build of the DC/OS artifacts and templates to the specified S3 bucket.
    ```bash
    $ sudo bash dcos_generate_config.sh --aws-cloudformation
    ```

1. The root URL for this bucket location is printed at the end of this step. You should see a message like this:
    ```
    AWS CloudFormation templates now available at: https://<amazon-web-endpoint>/<path-to-directory>
    ```
    
## Create Dependencies

Use the `zen.sh` script to create the Zen template dependencies. These dependencies will be used as input to create your stack in CloudFormation.

1. Save the following script as zen.sh.

    ```bash
    #!/bin/bash
    export AWS_DEFAULT_OUTPUT="json"
    set -o errexit -o nounset -o pipefail

    if [ -z "${1:-}" ]
    then
      echo Usage: $(basename "$0") STACK_NAME
      exit 1
    fi

    STACK_NAME="$1"
    VPC_CIDR=10.0.0.0/16
    PRIVATE_SUBNET_CIDR=10.0.0.0/17
    PUBLIC_SUBNET_CIDR=10.0.128.0/20

    echo "Creating Zen Template Dependencies"

    vpc=$(aws ec2 create-vpc --cidr-block "$VPC_CIDR" --instance-tenancy default | jq -r .Vpc.VpcId)
    aws ec2 wait vpc-available --vpc-ids "$vpc"
    aws ec2 create-tags --resources "$vpc" --tags Key=Name,Value="$STACK_NAME"
    echo "VpcId: $vpc"

    ig=$(aws ec2 create-internet-gateway | jq -r .InternetGateway.InternetGatewayId)
    aws ec2 attach-internet-gateway --internet-gateway-id "$ig" --vpc-id "$vpc"
    aws ec2 create-tags --resources "$ig" --tags Key=Name,Value="$STACK_NAME"
    echo "InternetGatewayId: $ig"

    private_subnet=$(aws ec2 create-subnet --vpc-id "$vpc" --cidr-block "$PRIVATE_SUBNET_CIDR" | jq -r .Subnet.SubnetId)
    aws ec2 wait subnet-available --subnet-ids "$private_subnet"
    aws ec2 create-tags --resources "$private_subnet" --tags Key=Name,Value="${STACK_NAME}-private"
    echo "Private SubnetId: $private_subnet"

    public_subnet=$(aws ec2 create-subnet --vpc-id "$vpc" --cidr-block "$PUBLIC_SUBNET_CIDR" | jq -r .Subnet.SubnetId)
    aws ec2 wait subnet-available --subnet-ids "$public_subnet"
    aws ec2 create-tags --resources "$public_subnet" --tags Key=Name,Value="${STACK_NAME}-public"
    echo "Public SubnetId: $public_subnet"
    ```

1. Run the script from from your local terminal with a single argument of stack prefix (STACK_NAME).
    ```bash
    $ bash ./zen.sh dcos
    ```
    
    The output should look like this:
    
    ```
    Creating Zen Template Dependencies
    VpcId: vpc-e0bd2c84
    InternetGatewayID: igw-38071a5d
    Private SubnetId: subnet-b32c82c5
    Public SubnetId: subent-b02c55c4
    ```
    
1. Use these dependency values as input to create your stack in CloudFormation in the next steps.

## Deploy the CF template

1. Go to [CloudFormation](https://console.aws.amazon.com/cloudformation/home) and click **Create Stack**.

1. On the **Select Template** page, specify the custom template that you created on your master node via the `sudo bash dcos_generate_config.sh --aws-cloudformation` command . For example, `
https://s3-us-west-2.amazonaws.com/joel-test-gpu/templates/dcos/config_id/fe4f3d47e23eda85301c875a28a34a8f48e44f95/cloudformation/el7-zen-1.json`.

https://s3-us-west-2.amazonaws.com/suzanne-gpu/templates/dcos/config_id/5686c47130682abf7624b79ec854a68c969fbd11/cloudformation/el7-zen-1.json

1. On the **Specify Details** page, specify these values and and click **Next**.
    - **StackName** - Specify your cluster name.
    - **AdminLocation** - Accept default.
    - **CustomAMI** - Specify the custom AMI for your region:

      us-west-2: `ami-9b5d97fb`
      us-east-1: `ami-e10e50f6`
      ap-southeast-2: `ami-37b28f54`

    - **InternetGateway** - Specify the InternetGatewayId value from your zen template dependencies (e.g. `igw-f15f0895`).
    - **KeyName** - Specify your AWS EC2 key pair.
    - **MasterInstanceType** - Accept the default master instance type (e.g. `m3.xlarge`).
    - **OAuthEnabled** - Indicate whether to enable OAuth.
    - **PrivateAgentInstanceCount** - Specify the number of private agent nodes.
    - **PrivateAgentInstanceType** - Specify a machine type of `g2.2xlarge`. This is the GPU-supported machine type.
    - **PrivateSubnet** - Specify the Private SubnetId value from your zen template dependencies (e.g. `subnet-e7193fbf`).
    - **PublicAgentInstanceCount** - Specify the number of public agent nodes.
    - **PublicAgentInstanceType** - Specify a machine type of `g2.2xlarge`. This is the GPU-supported machine type.
    - **PublicSubnet** - Specify the Public SubnetId value from your zen template dependencies (e.g. `subnet-98193fc0`).
    - **Vpc** - Specify the VpcId value from your zen template dependencies (e.g. `vpc-49a0202e`).

1. On the **Options** page, accept the defaults and click Next.
    
    **Tip**: You can choose whether to rollback on failure. By default this option is set to **Yes**.

1. On the **Review** page, check the acknowledgement box, then click **Create**.
    
    **Tip**: If the **Create New Stack** page is shown, either AWS is still processing your request or you’re looking at a different region. Navigate to the correct region and refresh the page to see your stack.