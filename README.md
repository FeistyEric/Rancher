# Rancher
This is to create a rancher server in Hetzner Cloud so that we can build the Kubernetes cluster.

## 1. Create a network

In Hetzner cloud, create a new network, we'll call it `devops`

## 2. Create an SSH key

1. Create an RSA Key Pair
`ssh-keygen -b 1024 -t rsa -f id_rsa`

Enter a passphrase

This will create `id_rsa` and `id_rsa.pub`

2. Add the identify to your host
`ssh-add id_rsa`

3. Add the id_rsa.pub contents to the SSH keys in Hetzner.


## 3. Create the rancher container

Rancher via Docker requires 4GB RAM minimum. This is CX21 for €4.90/m. Choose Nuremberg as the datacenter for the lowest ping.

1. Select Nuremberg
2. Select Ubuntu (latest LTS)
3. Standard type
4. Select CX21 - 4GB for €4.90/m
5. Select `devops` network
6. Select the ssh key
7. Call it `rancher`

## 4. Point a DNS record to the new server

In this case, we'll point `rancher.commersive.dev` to the IP address of the `rancher` server.

## 5. Install Rancher

1. SSH to the server using root@rancher.commersive.dev and accept the fingerprint. You should access immediately as the keys have been set up.
2. Install docker
```bash
apt-get update
apt install -y docker.io
systemctl start docker
```

3. Install Rancher as an automatically restarting docker container.

```bash
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /root/rancher:/var/lib/rancher \
  --name rancher-server \
  rancher/rancher:latest \
  --acme-domain rancher.xxxx
```

4. Access rancher at:
https://rancher.xxx

5. Set up an account with a STRONG password. The username is `admin`.

6. Store the password in Keypass
