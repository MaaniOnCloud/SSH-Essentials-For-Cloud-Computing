# SSH Essentials For Cloud:cloud:Computing 

### 1. Basic Connections & Options

##### - Simple Connnection
For a simple connection, you can use the following command:

` ssh user@hostname `

*hostname* could be the local or public IP address or domain name of the server.

##### - Connection with a different port
By Default SSH uses port 22. If you want to establish a ssh connection using a different port, you can use the following command:
` ssh user@hostname -p port_number `

##### - Connection with Verbose Mode
If you want to see what is happening behind the scenes, you can use the following command:
` ssh user@hostname -v `

*-v* is the verbose option.
The client will print the debugging information about the connection that is being established and even at the time of ending the connection.

##### - Connection with X11 Forwarding 
If you want to open X11 apps on the remote machine, X11 forwarding comes in real handy. 
While establishing the connection, you the -X option like below:

` ssh user@hostname -X `

And once you are connected, you can run a command like:

>`xclac` - for  graphical calculator
><br>
>`xclock` - for a clock
><br>
>`gedit` - for a text editor

In order for this to work, your server must have X11 forwarding enabled, which I will cover in the next coming section. Your client machine must have X11 server running. If you are on Linux, you are good. If you are on Mac, you would need something like **XQuartz**. If you are on Windows, you would need something like **Cygwin** or **Xming**.I personally don't use this feature at all. But its good to know.

##### - Connection and Execution of Command(s)
If you want to just execute a quick command or commands but dont want to have a shell session open, you can use the following command:
`ssh user@hostname [command]`
<br>
`ssh user@hostname uptime`

By using double quotes and semicolon you can execute multiple commands. Here is an example:
`ssh user@hostname "uptime; ls -l; pwd; whoami; hostname"`

##### - Connection with a Key  
Usually we generate a key pair, keep the private key with us and the public key on our server. However sometimes, eg: AWS, when you want to connect to your ec2 instance, you need to provide your private key to establish the connection.
So in that case, you can use the following command:
` ssh user@hostname -i path_to_your_private_key `
 
 The easier way is to add the key to your ssh agent.
 First you need to make sure the ssh agent is running on your machine.
`ssh-agent`
If its not running, you can start it by: `eval "$(ssh-agent -s)"`
Then you can add your key to the ssh agent by running: 
`ssh-add path_to_your_private_key`

##### - Creating a SSH Config File
If you are sick of typing all your options and the name of the server, you can create a config file and store all your options in it.
You can create a config file in the .ssh folder.
Eg:
```bash
Host mail_server
    Hostname 192.168.229.135
    User your_username
    Port 8455
    IdentityFile path_to_your_private_key
```
Once you have your config file ready, you can simply establish a ssh connection by typing `ssh host` or in our case `ssh mail_server`.

### 2. Generating & Managing SSH Keys

#### - Generating SSH Keys
Usually when you establish a ssh connection, it will ask you for your password. But passwords are not the best security measure. That is why SSH key pairs are used. You can generate the key pair by running the following command:
`ssh-keygen`
And it will generate a private key and a public key. And as the name suggests, **Private Key** is to be kept with you and **Public Key** is to be stored on the server.
You can also generate key pairs with different algorithms, and lenghts. And for that you need to specify the algorithm and the length using the -t and -b options.
```bash
ssh-keygen -t rsa -b 2048
ssh-keygen -t dsa -b 4096
ssh-keygen -t ecdsa -b 3072
ssh-keygen -t ed25519 
```
In the above example, we have used rsa, dsa, ecdsa and ed2551 alogrithms respectively and for each of them we have used a key length of 2048, 4096, 3072 and default respectively.

Default location of the keys is in the ~/.ssh folder on Linux and Mac. On Windows, it is in the C:\Users\username\.ssh folder.

#### - Copying Public Keys to the Server
Once you are done generating a key pair, your public key which called **id_rsa.pub** will be stored in the ~/.ssh folder of the server for the authentication.
You can copy the public key to the server by running the following command:
> `ssh-copy-id ~/.ssh/id_rsa.pub user@hostname`

This command will copy the contents of your public key to the ~/.ssh folder of the server in a file called authorized_keys.
Let's say you can't copy the public key to the server using the above command. You can do it manually by running the following command:

>`cat ~/.ssh/id_rsa.pub | ssh user@hostname 'cat >> ~/.ssh/authorized_keys'`

