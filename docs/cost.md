# Cost Model

## Overview

This is a reference cost model. Actual costs vary by usage, region, and configuration.

## Key Cost Drivers

- Three separate KMS customer-managed keys cost ~$3/month each (~$36/year base) plus per-API-call charges — negligible at low scale, but at high S3 PUT/GET throughput the KMS API call costs can exceed the key fee itself; a shared key would be cheaper but collapses the audit isolation boundary [inferred]
- CloudTrail data events on S3 (object-level logging) add cost proportional to request volume — enabling them on all buckets in a high-throughput pipeline can cost hundreds of dollars per month, so selective enabling per sensitive bucket is the right default [inferred]
- VPC-isolated Lambda or EC2 compute adds ENI attachment latency and requires NAT Gateway or VPC endpoints for AWS API calls; skipping VPC endpoints and using NAT Gateway routes all KMS/S3 traffic through NAT, adding both cost and a single egress choke point [inferred]

## Estimated Monthly Cost

| Component | Dev (₹) | Staging (₹) | Production (₹) |
|-----------|---------|-------------|-----------------|
| Compute   | ₹2,000–5,000 | ₹8,000–15,000 | ₹25,000–60,000 |
| Database  | ₹1,500–3,000 | ₹5,000–12,000 | ₹15,000–40,000 |
| Networking| ₹500–1,000   | ₹2,000–5,000  | ₹5,000–15,000  |
| Monitoring| ₹200–500     | ₹1,000–2,000  | ₹3,000–8,000   |
| **Total** | **₹4,200–9,500** | **₹16,000–34,000** | **₹48,000–1,23,000** |

> Estimates based on ap-south-1 (Mumbai) pricing. Actual costs depend on traffic, data volume, and reserved capacity.

## Cost Optimization Strategies

- Use Savings Plans or Reserved Instances for predictable workloads
- Enable auto-scaling with conservative scale-in policies
- Use DynamoDB on-demand for dev, provisioned for production
- Leverage S3 Intelligent-Tiering for infrequently accessed data
- Review Cost Explorer weekly for anomalies
