Route53 provides DNS as service

DNS --> Domain Name System

Amazon Route 53 is AWS’s scalable Domain Name System (DNS) web service. It’s used to manage domain names and route internet traffic to resources like websites, web apps, and other AWS services. The name comes from the TCP/UDP port 53, which is used for DNS.

Here’s a breakdown in simple terms:

## Key Functions of Route 53

### 1. Domain Registration

- You can buy and manage domain names directly through AWS.

- Example: `mywebsite.com.`

### 2. DNS Service

- Translates human-readable domain names into IP addresses (e.g.,mywebsite.com → 192.0.2.44).

- Works for websites, email servers, and other internet resources.

### 3. Traffic Routing

- Controls how traffic is routed to your resources.

- Supports several routing policies:

  - Simple routing: Single resource for a domain.

  - Weighted routing: Split traffic across multiple resources.

  - Latency-based routing: Send users to the resource with the lowest latency.

  - Failover routing: Redirect users if a resource becomes unavailable.

  - Geolocation routing: Send traffic based on user location.

### 4. Health Checks & Monitoring

- Route 53 can monitor the health of your endpoints.

- Automatically route traffic away from unhealthy endpoints.

## Why it’s useful

- Integrates seamlessly with other AWS services like EC2, S3, and CloudFront.

- Scalable and highly available (designed for AWS global infrastructure).

- Can manage both AWS-hosted and external domains.

## 💡 Example:

You host a website on an EC2 instance. Using Route 53, you can:

1. Buy example.com.

2. Point it to your EC2’s IP.

3. Set a failover so if that EC2 goes down, traffic goes to a backup server.
