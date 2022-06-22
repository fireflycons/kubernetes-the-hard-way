# Installing the Client Tools

First identify a system from where you will perform administrative tasks, such as creating certificates, kubeconfig files and distributing them to the different VMs.

If you are on a Linux laptop, then your laptop could be this system. In my case I chose the master-1 node to perform administrative tasks. Whichever system you chose make sure that system is able to access all the provisioned VMs through SSH to copy files over.

## Access all VMs

Generate Key Pair on master-1 node

```bash
ssh-keygen
```

Leave all settings to default (hit ENTER at each prompt).

View the generated public key ID with:

```bash
cat .ssh/id_rsa.pub
```

> output (truncated)
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD......8+08b vagrant@master-1
```

Move public key of master to all other VMs. Using the bash `printf` command, we create a block of script to be executed at all the other VMs that will add our new public key.

```bash
printf "\ncat >> ~/.ssh/authorized_keys <<EOF\n$(cat .ssh/id_rsa.pub)\nEOF\n\n"
```

> output is similar to this

```bash
cat >> ~/.ssh/authorized_keys <<EOF
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD......8+08b vagrant@master-1
EOF
```

Copy the output of the above command into your paste buffer.
Then from your main workstation, use `vagrant ssh` to connect to each of the four other VMs in turn, and at each paste the script block.

Finally add the public key to `authorized_keys` on `master-1` so that we can ssh/scp to ourself. This is necessary otherwise the certificate distribution in the next step won't work

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

## Install kubectl

The [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl). command line utility is used to interact with the Kubernetes API Server. Download and install `kubectl` from the official release binaries:

Reference: [https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Linux

```bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.21.0/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### Verification

Verify `kubectl` version 1.21.0 or higher is installed:

```bash
kubectl version
```

> output

```
Client Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.0", GitCommit:"cb303e613a121a29364f75cc67d3d580833a7479", GitTreeState:"clean", BuildDate:"2021-04-08T16:31:21Z", GoVersion:"go1.16.1", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

It is OK that we get a connection refused here - we have not yet bootstrapped the control plane.

Next: [Certificate Authority](04-certificate-authority.md)
