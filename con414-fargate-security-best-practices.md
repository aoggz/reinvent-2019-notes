# CON 414: Security best practices for AWS Fargate

## Takeaways

- Look to incorporate savings plans
- Explore logging options other than CloudWatch
- Update task definition to secure ulimits & Linux capabilities
- Re-explore WAF

## Notes

### Cluster

- can scope IAM policies to Cluster

### Limitations

- Optimize image layers to prevent running out of space
  - multi-stage builds help

### ECR image scanning

- uses clair
- only OS package management dependencies (i.e. not npm, pip, nuget, etc.)

### Logging

- New:
  - fluentbit (has Kinesis firehose plugin)
  - FireLens
- CloudWatch

### Docker settings for security

- set ulimits
- remove linux capabilities

### Runtime analysis tooling

- Good for forensics
- Capture what's happening in containers
  - Can trigger based on suspicious activity
- Options:
  - sysdig
  - twistlock
  - newvector
  - X-Ray (ish)

### Instrument now to be ready for anything in the future

### WAF

- SQL injections
- Regional acces restrictions
- Lots of third-party rules
  - going to greatly expand no.

### GuardDuty

- role escalation

### CloudWatch anomoly detection

### CloudWatch logs filter metrics

### Commonly requested Fargate features/enhancements

- Persistent storage
  - EFS-based
- GPU support

github.com/aaitha
