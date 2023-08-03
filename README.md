# CloudWatch-Agent-Configuration-on-Amazon-Linux-and-Ubuntu
Fetches metrices for RAM and storage
1. ### Inside IAM dashboard, go to `Roles`: Generic steps for both instance types
   - In step 1, make sure to choose `AWS Service` and `EC2` respectively for `Trusted entity type` & `Use case`
   - In step 2, choose `CloudWatchAgentAdminPolicy` and `AmazonSSMManagedInstanceCore`
   - In step 3, name the role `CloudWatchAgentAdminRole`, review and hit `Create`
2. ### Go to Amazon Systems Manager Dashboard:  Generic steps for both instance types
   - Go to `Paramater Store` under `Application Management`
   - Hit `Create Parameter`
   - Give the following name: `/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json`
   - Select `Standard` tier
   - Choose `String` as a Type
   - Choose `text` as type
   - Paste the following `json` data under value:
  ```
{
	"metrics": {
		"append_dimensions": {
			"InstanceId": "${aws:InstanceId}"
		},
		"metrics_collected": {
			"mem": {
				"measurement": [
					"mem_used_percent"
				],
				"metrics_collection_interval": 300
			},
            "disk": {
				"measurement": [
                     "disk_used_percent"
				],
				"metrics_collection_interval": 300
			}
		}
	}
}
```
- Add tags if required and hit on create button.
### 3. On Amazon Linux:
  - Run `sudo yum install amazon-cloudwatch-agent` on the termial of your EC2 instance
  - Follow these steps to check and configure now:
   1.  The CloudWatch Agent is not in the running/configured state upon installation, therefore first check if it is running by using this command: `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status`
   2.  If not running/configured, then do this to start the CloudWatch Agent: `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a start`
   3.  Repeat step 1 to check again.
  - Attach IAM Role created in step 1 to your instance from EC2 dashboard > Security
### 4. On Ubuntu:
- Download from the link below the preferred CloudWatch Agent using command link tailored as per the Ubuntu architecture
- This step is already done below for `x86_64` Ubuntu architecture:
  1. `wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb` this downloads the CloudWatch Agent file from AWS S3 Bucket.
  2. `sudo dpkg -i -E ./amazon-cloudwatch-agent.deb` this installs the .DEB package.
  3. The CloudWatch Agent is not in the running state upon installation, therefore check first if it is running by using this command: `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status`
  4. If not running/configured, then do this: `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a start`
  5. Repeat step 3 to check again.
- Attach IAM Role created in step 1 to your instance from EC2 dashboard > Security

### 5. Go to CloudWatch dashboard in AWS
- Check custom namespace called `CWAgent`inside `All Metrics`

#### Note:
- [Detailed explnation for signature verification and CloudWatch Agent installation for all instances types](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html#download-CloudWatch-Agent-on-EC2-Instance-commandline-first)

:+1:Good luck. END  :shipit:
