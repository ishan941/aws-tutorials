# AWS commands for start and jenkins

- Create EC2 instance in aws
- You will get a public and private ip

### In the Terminal

    ssh -i <.pem> ubuntu@<public ip>

- At first is is too open so. We will secure

       chmod 600 <filename>

- Try it again

### Now

- Root user

      sudo su -

- Install java

      sudo apt install fontconfig openjdk-21-jre

- Check java version

      java --versiona

- Install jenkins

      sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
      https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
      echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
      https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null
      sudo apt-get update
      sudo apt-get install jenkins

- Check jenkins status

       systemctl status jenkins

## Check and test

#### 1. Open Browser

#### 2. Search for `<public ip>:8080`

- it doesnt open because we have to open it.

#### 3. Go to `ASW>EC2>instances>security`

- inbound rule

  `It is a request comming inside the asw to ec2 instance`

- Outbound rune

  `It is a request going putside the aws from ec2 instance`

#### 4. Click on security group

#### 5. Edit inbound traffic rule

- add rule
- set port 8080
- ipv4
- save changes

#### Now refresh the browser of jenkinsT

- It will ask for the crediantial

The crediantial is saved inside
`/var/lib/jenkins/secrets/initialAdminPassword`

To get it inside terminal

    cat /var/lib/jenkins/secrets/initialAdminPassword

And you will the the. password copy then paste it
