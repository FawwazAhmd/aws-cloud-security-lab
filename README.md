# AWS Cloud Security & Security Monitoring Lab

A practical AWS cloud security lab implementing **S3 security, CloudTrail data-event monitoring, CloudWatch detection, and automated SNS alerting**.

The project demonstrates an end-to-end security monitoring workflow where an S3 object upload is detected, logged, converted into a CloudWatch metric, evaluated by a security alarm, and reported through an automated email notification.

## Architecture

**S3 Object Upload → CloudTrail → CloudWatch Logs → Metric Filter → CloudWatch Metric → CloudWatch Alarm → SNS → Email Alert**

## Project Objectives

- Configure a secure Amazon S3 bucket
- Enable S3 object-level monitoring using AWS CloudTrail
- Centralize CloudTrail events using CloudWatch Logs
- Detect `PutObject` activity using a CloudWatch metric filter
- Trigger a CloudWatch security alarm when the detection threshold is reached
- Send automated security notifications using Amazon SNS
- Build and validate an end-to-end cloud security monitoring workflow

## AWS Services Used

| AWS Service | Purpose |
|---|---|
| **Amazon S3** | Secure storage and monitored object activity |
| **AWS CloudTrail** | S3 object-level audit logging |
| **Amazon CloudWatch Logs** | Centralized security event logging |
| **CloudWatch Metric Filter** | Detection of `PutObject` events |
| **CloudWatch Alarm** | Security threshold monitoring |
| **Amazon SNS** | Automated email alerting |

## Security Controls

The S3 bucket was configured with:

- **Block Public Access**
- **Bucket Owner Enforced** object ownership
- **ACLs disabled**
- **Versioning enabled**
- **SSE-S3 server-side encryption**
- **S3 Bucket Key enabled**

CloudTrail was configured with targeted S3 data-event logging for the lab bucket.

## Detection Workflow

### 1. S3 Object Upload

A test object is uploaded to the monitored S3 bucket, generating a `PutObject` event.

### 2. CloudTrail Logging

CloudTrail captures the S3 object-level activity as a data event.

### 3. CloudWatch Logs

The CloudTrail event is delivered to:

`/aws/cloudtrail/security-lab`

### 4. Metric Filter

The following CloudWatch metric filter detects `PutObject` events:

`{ $.eventName = "PutObject" }`

The filter publishes the custom metric:

`SecurityLab / S3PutObjectCount`

### 5. CloudWatch Alarm

The `S3-PutObject-Security-Alert` alarm monitors the custom metric and triggers when the configured threshold of **1 event within 5 minutes** is reached.

### 6. SNS Notification

When the alarm enters the **ALARM** state, Amazon SNS sends an automated email notification containing the alarm details.

## Evidence

The project was tested successfully using an actual S3 object upload. The resulting CloudTrail event was captured in CloudWatch Logs, detected by the metric filter, used to trigger the CloudWatch alarm, and followed by an SNS email notification.

The repository contains supporting screenshots demonstrating each stage of the workflow.

## Documentation

📄 **[View the Full AWS Cloud Security Lab Report](./AWS-Cloud-Security-Monitoring-Lab-Report.pdf)**

The full report contains:

- S3 security configuration
- CloudTrail data-event configuration
- CloudWatch log integration
- `PutObject` event evidence
- Metric filter configuration
- CloudWatch alarm configuration
- SNS email alert
- End-to-end architecture
- Security and cost considerations

## Skills Demonstrated

**Cloud Security • AWS • S3 Security • CloudTrail • CloudWatch • Security Monitoring • Threat Detection • Log Analysis • Security Alerting • SNS • Cloud Architecture**

## Cost & Security Considerations

CloudTrail data-event monitoring was scoped specifically to the lab S3 bucket to avoid unnecessary event collection. An AWS budget alert was also configured to monitor unexpected spending.

Sensitive information such as credentials, account identifiers, email addresses, and raw CloudTrail logs should not be committed to the public repository.

## Disclaimer

This project was developed as an educational cybersecurity lab using a personal AWS environment. Test events and configurations were created specifically for security monitoring and detection purposes.
