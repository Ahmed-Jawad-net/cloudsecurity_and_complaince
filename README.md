cat > README.md << 'EOF'
# Task 2 — Cloud Security & Compliance
## Internee.pk Cybersecurity Internship

### Objective
Ensure Internee.pk's cloud-based platforms follow industry-standard security measures.

### What was implemented

| Deliverable | Tool | Description |
|---|---|---|
| IAM Policies | AWS IAM | Users, groups, least privilege access control |
| Audit Logging | AWS CloudTrail | Multi-region trail logging to S3 |
| Multi-Region Backup | AWS S3 | Cross-region replication Mumbai → Singapore |
| WAF Protection | AWS WAFv2 | SQL injection, XSS, rate limiting rules |

### Lab Environment
- **Tool:** LocalStack 2026.6.1 (AWS simulation via Docker)
- **AWS CLI:** v2.35.15
- **OS:** Ubuntu 24.04
- **Region:** ap-south-1 (Mumbai)

### Key Commands Used

```bash
# IAM — create users and groups
awslocal iam create-group --group-name internee-developers
awslocal iam create-user --user-name jawad-admin
awslocal iam attach-user-policy --user-name jawad-admin \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

# CloudTrail — enable audit logging
awslocal cloudtrail create-trail \
  --name internee-audit-trail \
  --s3-bucket-name internee-audit-logs \
  --is-multi-region-trail
awslocal cloudtrail start-logging --name internee-audit-trail

# S3 — multi-region backup
awslocal s3api create-bucket --bucket internee-primary-data \
  --region ap-south-1
awslocal s3api create-bucket --bucket internee-backup-singapore \
  --region ap-southeast-1
awslocal s3api put-bucket-replication \
  --bucket internee-primary-data \
  --replication-configuration file://replication.json

# WAF — web application firewall
awslocal wafv2 create-web-acl \
  --name internee-web-acl \
  --scope REGIONAL \
  --default-action Allow={} \
  --rules file://waf-rules.json
```

### Results
All 4 deliverables completed and verified:
- ✅ 2 IAM users + 1 group + least privilege policies
- ✅ CloudTrail multi-region trail active (IsLogging: true)
- ✅ S3 cross-region replication Mumbai → Singapore
- ✅ WAF blocking SQL injection + XSS + rate limiting

### Intern
**Jawad Ahmed** — Cybersecurity Intern, Internee.pk
EOF
