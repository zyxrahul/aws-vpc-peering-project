# AWS VPC Peering — Private Connectivity Between Two VPCs

**EWB Courses · AWS Architect Associate**  
**Student:** Rahul Kumar  
**Batch/Class:** AWS

## Project Overview

This project demonstrates how to establish private network connectivity between two non-overlapping Amazon Virtual Private Clouds (VPCs) using AWS VPC Peering.

The planned architecture includes a bastion host for administrative access, two private EC2 workloads, explicit route tables, restricted security group rules, and connectivity tests.

> **Current status:** The AWS CloudFormation stack has been created successfully. The initial deployment has `EnablePeering=false`. Full connectivity verification, evidence collection, and final report completion are still pending.

## Project Architecture

The project uses the following private address plan:

| Resource | CIDR / Private IP |
|---|---|
| VPC A | `10.10.0.0/16` |
| VPC A public subnet | `10.10.1.0/24` |
| VPC A private subnet | `10.10.2.0/24` |
| VPC B | `10.20.0.0/16` |
| VPC B private subnet | `10.20.1.0/24` |
| Bastion | `10.10.1.10` plus public IPv4 |
| App-A | `10.10.2.10`, private IPv4 |
| App-B | `10.20.1.10`, private IPv4 |

App-A and App-B are intended to communicate through private IP addresses after the peering connection and required routes are configured.

## AWS Deployment Information

| Property | Current value |
|---|---|
| CloudFormation stack | `ewb-vpc-peering-lab` |
| AWS Region | Asia Pacific (Sydney) — `ap-southeast-2` |
| CloudFormation status | `CREATE_COMPLETE` |
| EnablePeering parameter | `false` at last verification |
| VPC A ID | `vpc-08d5228a56eb0a849` |
| VPC B ID | `vpc-0224bdbe81ba1233a` |
| Bastion instance ID | `i-0fd9ef6d35d04a962` |
| Bastion public IPv4 | `3.27.158.236` |
| App-A instance ID | `i-09729698ff15a83af` |
| App-A private IPv4 | `10.10.2.10` |
| App-B instance ID | `i-05503cc6610e934e8` |
| App-B private IPv4 | `10.20.1.10` |
| Peering connection ID/status | Pending verification |
| GitHub repository URL | Add after repository creation |

Resource states and IP addresses should be rechecked before final submission because they can change.

## Repository Contents

- `infrastructure/vpc-peering.yaml` — AWS CloudFormation template.
- `report/AWS_VPC_Peering_Submission_Draft.pdf` — formatted report draft.
- `report/main.typ` — editable Typst report source.
- `report/architecture.svg` — architecture diagram.
- `report/architecture.mmd` — Mermaid diagram source.
- `evidence/README.md` — required screenshot checklist.
- `student-info.md` — student and resource information.
- `preview/` — architecture preview assets.

## Deployment and Verification Plan

1. Review the existing CloudFormation stack and verify its resources and parameters.
2. Confirm that the SSH security group allows access only from the current authorized IPv4 `/32`.
3. Establish administrative access to App-A through the bastion.
4. Capture the baseline connectivity result while peering is disabled.
5. Update the existing CloudFormation stack to set `EnablePeering=true`.
6. Verify that the peering connection is `Active` and both VPC route tables contain the required routes.
7. Test forward and reverse private connectivity between App-A and App-B.
8. Test private SSH access to App-B through the bastion, if supported by the configured SSH client and key.
9. Perform the controlled ICMP failure and recovery tests, restoring the original security rule afterward.
10. Save genuine AWS console and terminal screenshots under `evidence/`.
11. Update the report with actual test results, observations, and completion date.
12. Delete the lab resources after preserving the required evidence, and verify cleanup.

Do not claim a test passed unless its actual output has been observed and recorded.

## Evidence Status

The deployment status has been verified from the CloudFormation console screenshot. The full connectivity test suite has not yet been verified.

Required evidence is documented in `evidence/README.md`. Capture original screenshots from your own AWS environment; do not fabricate or recreate screenshots of results that were not obtained.

## Cost and Security Considerations

- Review EC2, EBS, public IPv4, and other applicable AWS charges before continuing.
- Restrict bastion SSH access to the current authorized public IPv4 `/32`.
- Do not expose SSH to `0.0.0.0/0`.
- Do not commit private keys, passwords, AWS access keys, secret keys, or access tokens.
- Keep App-A and App-B without public IPv4 addresses.
- Remove lab resources when testing is complete and verify that no billable resources remain.

## Project Completion Criteria

The project will be considered complete after the peering connection, route configuration, forward and reverse connectivity tests, controlled failure and recovery tests, evidence collection, final report, and resource cleanup have been verified.

## Skills Demonstrated

- AWS VPC and subnet design
- VPC Peering and private routing
- Amazon EC2 and bastion-host architecture
- Security groups and restricted SSH access
- AWS CloudFormation (Infrastructure as Code)
- Network connectivity troubleshooting

## Project Status

**Status:** Infrastructure deployed; connectivity verification pending.

The project is being validated in AWS. Actual test results and screenshots will be added after verification.

## Author

**Rahul Kumar**  
AWS Learner | B.Tech CSE Student

- GitHub:- https://github.com/zyxrahul


