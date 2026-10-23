# AWS VPC Peering Student Project

**EWB Courses · AWS Architect Associate**  
**Goal:** privately connect two non-overlapping VPCs, administer both private EC2 workloads through a bastion, and demonstrate positive and controlled negative connectivity tests.

> **Honest status:** the report and infrastructure-as-code are prepared. This repository does not claim that AWS resources were deployed or tests were run. Before submitting as completed work, fill in your name/batch/resource IDs, deploy in your own lab account, run every test, add your own screenshots under `evidence/`, and replace the marked pending results in the report. Do not submit expected criteria as actual evidence.

## Repository contents

- `report/AWS_VPC_Peering_Submission_Draft.pdf` — formatted report with architecture, address plan, routes, security rules, test matrix and cleanup checklist. Student-specific and execution evidence fields still need completion.
- `report/main.typ` — editable report source.
- `report/architecture.svg` — clean architecture diagram using the workbook's CIDRs.
- `infrastructure/vpc-peering.yaml` — CloudFormation template for the VPCs, subnets, route tables, peering, security groups and three EC2 instances.
- `evidence/README.md` — screenshot checklist and naming suggestions.
- `student-info.md` — fields to complete before submission.

## Design address plan

| Resource | CIDR / private IP |
|---|---|
| VPC A | `10.10.0.0/16` |
| VPC A public subnet | `10.10.1.0/24` |
| VPC A private subnet | `10.10.2.0/24` |
| VPC B | `10.20.0.0/16` |
| VPC B private subnet | `10.20.1.0/24` |
| Bastion | `10.10.1.10` + public IPv4 |
| App-A | `10.10.2.10`, no public IPv4 |
| App-B | `10.20.1.10`, no public IPv4 |

The supplied hand sketch showed `28.x.x.x` addresses, which do not match the workbook's actual resource plan. This repository consistently follows the workbook: `10.10.0.0/16` and `10.20.0.0/16`.

## Deploy in your AWS lab (after instructor approval)

1. Use the same AWS account and Region, such as Mumbai `ap-south-1`. Confirm instance and public IPv4 pricing/credits before launch.
2. Create or choose an EC2 key pair in the target Region. Keep its private `.pem` locally; never commit it.
3. Find your current public IPv4 and use it as a `/32` value. Do not use `0.0.0.0/0` for bastion SSH.
4. Create the CloudFormation stack from `infrastructure/vpc-peering.yaml` with `EnablePeering=false` (the default). Supply `KeyPairName` and `MyPublicCidr`. This creates both VPCs, the three instances and the pre-peering network. Review resources/pricing first.
5. SSH to App-A through the bastion and capture `ping -c 4 10.20.1.10` as the baseline. It should receive no replies because peering and peer routes do not exist yet. Save this screenshot before proceeding.
6. Update the same stack parameter to `EnablePeering=true`. Wait for stack update; then verify peering is `Active` and peer routes are present. Run the forward/reverse pings, private SSH to App-B and controlled ICMP failure/recovery tests in `evidence/README.md`.
7. Capture your own console/terminal screenshots and add originals under `evidence/`. Confirm App-A and App-B have no public IPv4. Complete the report's actual-results cells and student fields; never put credentials in GitHub.
8. Save evidence, then delete the CloudFormation stack. Verify no project EC2 instances, volumes, Elastic IPs, route tables, security groups, peering connections or VPCs remain, and check billing.

### Example AWS CLI deployment

Use AWS CLI only if installed/configured in your environment. First create the stack with peering disabled to capture the required baseline. Replace the parameter values and review costs before launch.

```bash
aws cloudformation create-stack \
  --stack-name ewb-vpc-peering-lab \
  --template-body file://infrastructure/vpc-peering.yaml \
  --parameters ParameterKey=KeyPairName,ParameterValue=YOUR_KEY_PAIR \
               ParameterKey=MyPublicCidr,ParameterValue=YOUR_PUBLIC_IP/32 \
               ParameterKey=EnablePeering,ParameterValue=false
```

After capturing the baseline, update the same stack:

```bash
aws cloudformation update-stack \
  --stack-name ewb-vpc-peering-lab \
  --template-body file://infrastructure/vpc-peering.yaml \
  --parameters ParameterKey=KeyPairName,UsePreviousValue=true \
               ParameterKey=MyPublicCidr,UsePreviousValue=true \
               ParameterKey=EnablePeering,ParameterValue=true
```

Monitor with `aws cloudformation describe-stacks --stack-name ewb-vpc-peering-lab`. After saving evidence and verifying results, delete with `aws cloudformation delete-stack --stack-name ewb-vpc-peering-lab`.

## Evidence status

No real AWS console or terminal screenshots are included yet. Put your own screenshots in `evidence/` and record actual observations in the report. Suggested names and required proof are listed in `evidence/README.md`.

## GitHub upload

Create a repository under your GitHub account (private is appropriate while it contains student work), upload these files, and add only the repository link to the trainer's Google Form when it is shared. Keep the repository free of private keys, passwords, AWS access keys, account IDs if your instructor prohibits them, and unrelated personal data.
