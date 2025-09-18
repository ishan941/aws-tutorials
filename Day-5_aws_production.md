# About the project

This example demonstrates how to create a VPC that you can use for servuces in a production environment.

![alt text](image.png)

To imporve resiliency, you deploy the servers in two Availablility Zones, by using auto scaling group and an application load balancer. For additional security, you deploy the servers in private subnets. The servers recieve requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To imporve resiliency, you deploy the NAT gateway in both Availability Zones.

### Overview

The VPC has public subnets and private subnets in two Availability Zones.

Each public subnet contains a NAT gateway and a load balancer node.

The servers run in the private subnets, are launched and terminated by using an Auto Sacling group, and receive traffic from the load balancer.

The Servers can connect to the internet by using the NAT gateway.

#### Before we start

- Auto Scaling Group
- Load Balancer
  - It balance the load to the server
- Target Group
- Bastion HOst or Junp Server

### Let's get started

**1. Create VPC with more**

- Give a aws-prod-example
- 10.0.0.0/16 is `enough`
- No ipv6
- No of AZ 2
- NAT gateway --> 1 per AZ
- VPC endpoint --> None

**Create Auto Scalling Group**

_create Launch template_

- Name --> same as vpc
- des --> proof of concept for app deploy in aws private subnet
- create security group
- select vpc
- inbound security group
- type -> ssh
- type -> custom TCp
- port -> 8000

_Now use template_

- Chose vpc
- All private subnet
- No load balancer
- Instance -> 2
- Desired Capacity -> 2

Check if the instance has created or not
since public ip is not available so we will create bastion host

- Create instance
- name -> bastion-host
- add security group
- edit network
- public subnet

_Go to terminal copy .pem file to private instance_

    scp -i ~/Downloads/privates/aws/aws_login.pem ~/Downloads/privates/aws/aws_login.pem ubuntu@13.61.194.3:/home/ubuntu

- Login to instance
  ssh -i ~/Downloads/privates/aws/aws_login.pem ubuntu@<publicip>

_Login to private instacne_

- copy the private ip address

  ssh -i .pem ubuntu@<privateip>

- Create simple html file
  vim index.html
- paste it

  ```
  <!doctype html>
  <html lang="en">
  <head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Simple HTML Page</title>

  <!-- Tiny, clean CSS -->
  <style>
    :root {
      --bg: #f6f8fb;
      --card: #fff;
      --accent: #2563eb;
      --muted: #6b7280;
      --radius: 12px;
    }
    body {
      margin: 0;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: linear-gradient(180deg,var(--bg), #eef2f7 60%);
      color: #111827;
      line-height: 1.45;
      padding: 24px;
      display: flex;
      justify-content: center;
    }
    .container {
      max-width: 900px;
      width: 100%;
      background: var(--card);
      border-radius: var(--radius);
      box-shadow: 0 6px 18px rgba(16,24,40,0.08);
      padding: 28px;
    }
    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
    }
    h1 { margin: 0; font-size: 1.4rem; }
    nav a {
      color: var(--accent);
      text-decoration: none;
      margin-left: 12px;
      font-weight: 600;
    }
    main { margin-top: 18px; }
    .card {
      padding: 16px;
      border-radius: 10px;
      background: linear-gradient(180deg,#ffffff, #fbfdff);
      box-shadow: 0 4px 12px rgba(16,24,40,0.04);
      margin-bottom: 12px;
    }
    footer { margin-top: 18px; color: var(--muted); font-size: 0.9rem; }
    button {
      background: var(--accent);
      color: white;
      border: none;
      padding: 10px 14px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
    }
    /* small responsive tweak */
    @media (max-width: 520px) {
      body { padding: 12px; }
      header { flex-direction: column; align-items: flex-start; gap: 8px; }
    }
  </style>
  </head>
  <body>
  <div class="container" role="main">
    <header>
      <h1>Simple HTML Template</h1>
      <nav aria-label="Main navigation">
        <a href="#about">About</a>
        <a href="#examples">Examples</a>
      </nav>
    </header>

    <main>
      <section id="about" class="card">
        <h2 style="margin-top:0">What this page has</h2>
        <ul>
          <li>Responsive meta tag</li>
          <li>Minimal CSS (no frameworks)</li>
          <li>Accessible header, nav, and footer</li>
          <li>Small interactive button below</li>
        </ul>
      </section>

      <section id="examples" class="card">
        <h2 style="margin-top:0">Try this</h2>
        <p>Click the button to see a small JavaScript action:</p>
        <button id="greetBtn">Say hello</button>
        <p id="greetOutput" style="margin-top:10px;color:var(--muted)"></p>
      </section>
    </main>

    <footer>
      © <span id="year"></span> — Simple HTML example. Built for learning.
    </footer>
  </div>

  <!-- Tiny JS -->
  <script>
    // update year
    document.getElementById('year').textContent = new Date().getFullYear();

    // button interaction
    document.getElementById('greetBtn').addEventListener('click', function () {
      const out = document.getElementById('greetOutput');
      out.textContent = "Hello — you're viewing a simple HTML page! 🎉";
      // simple visual feedback
      this.textContent = "Clicked!";
      this.disabled = true;
      setTimeout(() => {
        this.textContent = "Say hello";
        this.disabled = false;
        out.textContent = "";
      }, 2000);
    });
  </script>
  </body>
  </html>

  ```

  save it `wq!`

run it:

    python3 -m http.server 8000

**Create Load balancer**

- Create loadbalancer
- name -> aws-pord-example
- internet facing
- select vpc
- pick up both subnet
- public subnet group

_create target group_

- select inatnce
- include pending group
- create target group
