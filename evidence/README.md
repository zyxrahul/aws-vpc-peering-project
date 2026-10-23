# Evidence to capture from your own AWS run

The folder intentionally contains no simulated or fabricated AWS screenshots. Add original screenshots here after you execute the lab. Blur account identifiers if your instructor does not require them; never include credentials, private keys, passwords or access tokens.

Suggested files:

1. `01-vpc-subnets.png` — both VPC CIDRs and subnet list.
2. `02-route-table-associations.png` — each of the three custom route tables and its subnet association.
3. `03-peering-active.png` — connection name/ID, `Active` status and both VPC IDs.
4. `04-security-groups.png` — restricted inbound rules; show bastion SSH as your current IPv4 `/32`.
5. `05-instances-private-ip.png` — all three instance names/private IPs and verify App-A/App-B have no public IPv4.
6. `06-baseline-fail.png` — App-A `ping -c 4 10.20.1.10` before peering routes; expected no replies.
7. `07-forward-ping-success.png` — same ping after routes/security groups; expected 4 replies, 0% loss.
8. `08-reverse-ping-success.png` — App-B `ping -c 4 10.10.2.10`; expected 4 replies, 0% loss.
9. `09-app-b-private-ssh.png` — laptop command `ssh ewb-app-b` and shell identity showing App-B reached through ProxyJump.
10. `10-controlled-icmp-failure.png` — after temporarily removing only App-B's ICMP Echo Request rule from App-A `/32`, ping should stop.
11. `11-rule-restored-recovery.png` — after restoring the exact rule, ping should recover.
12. `12-cleanup.png` — CloudFormation stack deletion complete and project resources cleared.

For each screenshot, label the test, host, and actual result. Keep the baseline screenshot from before peering/routes; do not recreate it after the fact. Record actual outputs in the report and explain any deviation from expected results.
