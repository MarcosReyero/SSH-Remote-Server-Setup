# SSH Access Setup on AWS Instance

The goal of this project is to configure a remote server on AWS, allowing SSH access using custom SSH keys and setting up an alias for easier connections.

## Requirements
- AWS account with a running EC2 instance.
- Access to your system’s terminal.
- SSH keys (you can generate new ones or use default ones).

## Steps to complete the project

### 1. Create the Instance on AWS
- Go to the AWS EC2 console and create a new instance.
- Make sure to select Ubuntu as the operating system.
- During the creation process, select or create an SSH key pair for the initial connection.
- Once the instance is created, note down the public IP address of the instance.

### 2. Generate a New SSH Key Pair
On your instance terminal, run the following command to generate a new key pair:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/key1
```
This will generate two files:
- `key1` (private key)
- `key1.pub` (public key)

During key generation, you’ll be prompted for an optional passphrase. If you prefer not to use one, leave the field empty.

### 3. Add the Public Key to authorized_keys
Next, add the public key (`key1.pub`) to the `authorized_keys` file to allow access with the new private key:

```bash
cat ~/.ssh/key1.pub >> ~/.ssh/authorized_keys
```

### 3. Add the Public Key to authorized_keys
Next, add the public key (`key1.pub`) to the `authorized_keys` file to allow access with the new private key:

```bash
cat ~/.ssh/key1.pub >> ~/.ssh/authorized_keys
```
Verify that the key is added correctly:

```bash
cat ~/.ssh/authorized_keys
```
### 4. Transfer the Private Key to Your Local Machine
Use scp to copy the private key from your AWS instance to your local machine:

``` bash
scp -i /path/to/my_test_key.pem ubuntu@18.216.239.51:~/.ssh/key1 ~/.ssh/
```

Make sure to replace /path/to/my_test_key.pem with the path to your original private key.

### 5. Change the Permissions of the Key
After transferring the key to your local machine, change its permissions to ensure that SSH can use it:

```bash
chmod 600 ~/.ssh/key1
```

### 6. Connect to the Instance Using the New SSH Key
You can now connect to your AWS server using the new private key (key1):

``` bash
ssh -i ~/.ssh/key1 ubuntu@18.216.239.51
```
### 7. Set Up an SSH Alias for Easier Connection
To simplify future connections, you can configure an SSH alias by editing the ~/.ssh/config file:

``` bash
nano ~/.ssh/config
```
Add the following configuration:

```ini
    Host my-aws-server
    HostName 18.216.239.51
    User ubuntu
    IdentityFile ~/.ssh/key1
```
Now, you can easily connect to your server using:

```bash
ssh my-aws-server
```
### 8. (Optional) Set Up Fail2Ban for Extra Security
As an optional step, you can install and configure fail2ban to protect your server from brute-force attacks. Run the following commands to install it:

```bash
sudo apt update
sudo apt install fail2ban
```
Then, configure fail2ban by editing its configuration file.

### Conclusion
With these steps, we successfully configured an AWS instance to allow SSH access using custom SSH keys. This project also covers how to set up an alias for easier connections and how to secure the server further with fail2ban.With these steps, we successfully configured an AWS instance to allow SSH access using custom SSH keys. This project also covers how to set up an alias for easier connections and how to secure the server further with fail2ban.
