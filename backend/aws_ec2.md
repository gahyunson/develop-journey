EC2 of AWS is a Single VPS.

# Set up do deploy
1. Create AWS account.
2. Create IAM user that has administrative privileges that you can use in your account.
3. Receive SSH key on the machine.
```bash
apple@GH-MacBook-Pro ~ % cd .ssh
apple@GH-MacBook-Pro .ssh % ls
id_rsa		id_rsa.pub	known_hosts	known_hosts.old
apple@GH-MacBook-Pro .ssh % ssh-keygen -t rsa -b 4096
...
apple@GH-MacBook-Pro .ssh % ls
aws_id_rsa	id_rsa		known_hosts
aws_id_rsa.pub	id_rsa.pub	known_hosts.old
```
aws_id_rsa: private key
aws_id_rsa.pub: public key

4. and copy this value,
```bash
cat aws_id_rsa.pub
```

5. EC2 - Keypairs - Import key pair
One key for each computer.
Two different machine -> a different key on each machine.

We can use this key to spin up new servers and automatically give access to the person who has this key to the server.

6. Create EC2 instance. (To deploy)
EC2 dashboard - Launch instance

    a. Writing Name
    b. check number of instance
    c. Amazon Linux (free tier eligible)
    d. Instance type - t2_mocro
    e. Key pair
    f. Key pair (what we create step 5.)
    g. Network settings Allow SSH traffic from Anywhere
    h. Configure storage
    i. Launch instance

7. Authentication via the key that we uploaded to the account.
    a. DashBoard - Instances - Public IPv4 address
    b. Copy the address and start command... `ssh-add aws_id_rsa`

```bash
apple@GH-MacBook-Pro .ssh % ssh-add aws_id_rsa
Enter passphrase for aws_id_rsa:
Identity added: aws_id_rsa (apple@GH-MacBook-Pro.local)
apple@GH-MacBook-Pro .ssh % ssh ec2-user@3
The authenticity of host '3 ()' can't be established.
ED25519 key fingerprint is
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '3' () to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
[ec2-user@ip- ~]$
```
    Now we allowed public access from the internet to our SSH server.

8. Public key on our remote server.
Set up a deploy key. we can imporove our server to be able to pull the code from GitHub so that we can deploy to the server.
```bash
cat ~/.ssh/id_ed25519.pub
```

The GitHub repository - settings - Deploy keys - Add new

# Set up our server with some of the dependencies
- `sudo yum install git -y`

- `sudo yum install docker -y`: install docker
- `sudo systemctl enable docker.service`: enable the docker service on the system
- `sudo systemctl start docker.service`: start docker service
- `sudo usermod -aG docker ec2-user`: It gives out easy to user permission to run docker containers.
- `exit`: exit from remote(aws) server. Log out.
- `ssh ec2-user@3.23532532`: Log in.

- `sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`: Download docker compose to our system.

- `sudo chmod +x /usr/local/bin/docker-compose`: give the permissions to run command.

# Clone on the remote server
```bash
[ec2-user@ip- ~]$ git clone git@github.com:gahyunson/recipe-app-api.git
```

```
[ec2-user@ip- ~]$ cd recipe-app-api/
[ec2-user@ip- recipe-app-api]$ ls
Dockerfile  app                        docker-compose.yml  requirements.dev.txt  scripts
README.md   docker-compose-deploy.yml  proxy               requirements.txt
[ec2-user@ip- recipe-app-api]$ ls -la

[ec2-user@ip- recipe-app-api]$ git pull origin
Already up to date.
[ec2-user@ip- recipe-app-api]$ cp .env.sample .env
[ec2-user@ip- recipe-app-api]$ vi .env
```

Set up the values.

# Run our service

```bash
docker-compose -f docker-compose-deploy.yml up -d
[29482] Error loading Python lib '/tmp/_MEIYqNtYm/libpython3.7m.so.1.0': dlopen: libcrypt.so.1: cannot open shared object file: No such file or directory
[ec2-user@ip- recipe-app-api]$ sudo yum install -y libxcrypt-compat
Installed:
  libxcrypt-compat-4.4.33-7.amzn2023.x86_64

Complete!
```

```bash
[ec2-user@ip-172-31-16-131 recipe-app-api]$ docker-compose -f docker-compose-deploy.yml up -d
```

# Stop the server
```bash
docker-compose -f docker-compose-deploy.yml down
```

# Upload file to remote server
1. Move to the repository folder and pull from github.
```bash
[ec2-user@ip- recipe-app-api]$ git pull origin
```

2. Rebuild including any other change.
```bash
[ec2-user@ip- recipe-app-api]$ docker-compose -f docker-compose-deploy.yml build app
```

3. Recreate our server, Restart out app but no dependencies. We already ran dependencies so we don't need to re-run.
```bash
[ec2-user@ip- recipe-app-api]$ docker-compose -f docker-compose-deploy.yml up --no-deps -d app
```

### If you want to see logs
docker-compose -f docker-compose-deploy.yml logs

### If you want to connect to aws.
Log in
```bash
apple@GH-MacBook-Pro .ssh % ssh-add aws_id_rsa
Enter passphrase for aws_id_rsa:
Identity added: aws_id_rsa (apple@GH-MacBook-Pro.local)
```

Connect to the server
```bash
apple@GH-MacBook-Pro .ssh % ssh ec2-user@3.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
[ec2-user@ip- ~]$
```
