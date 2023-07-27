# CloudWatch-Agent-Configuration-on-Amazon-Linux-and-Ubuntu
Fetches metrices for RAM and storage
## Generic steps (for both Amazon Linux and Ubuntu)
1. Inside IAM dashboard, go to `Roles`:
   - In step 1, make sure to choose `AWS Service` and `EC2` respectively for `Trusted entity type` & `Use case`
   - In step 2, choose `CloudWatchAgentAdminPolicy` and `AmazonSSMManagedInstanceCore`
   - In step 3, name the role `CloudWatchAgentAdminRole` review and hit `Create`
2. Go to Amazon Systems Manager Dashboard:
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
- Add tags if required an hit on create button.
3. On Amazon Linux:
  - Run `sudo yum install amazon-cloudwatch-agent` on the termial of your EC2 instance
  - Attach IAM Role created in step 1 to your instance from EC2 dashboard > Security
4. On Ubuntu
- Follow the steps from here:
- [Download and configure the CloudWatch agent using the command line](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html#download-CloudWatch-Agent-on-EC2-Instance-commandline-first)
- Attach IAM Role created in step 1 to your instance from EC2 dashboard > Security
 :+1:Good luck. END  :shipit:
