# SSH Essentials For Cloud:cloud:Computing 

### 1. Basic Connections & Options

###### - Simple Connnection
For a simple connection, you can use the following command:
` ssh user@hostname `

*hostname* could be the local or public IP address or domain name of the server.

###### - Connection with a different port
By Default SSH uses port 22. If you want to establish a ssh connection using a different port, you can use the following command:
` ssh user@hostname -p port_number `

###### - Connection with Verbose Mode
If you want to see what is happening behind the scenes, you can use the following command:
` ssh user@hostname -v `
*-v* is the verbose option.
The client will print the debugging information about the connection that is being established and even at the time of ending the connection.

###### - Connection with X11 Forwarding 
If you want to open X11 apps on the remote machine, X11 forwarding comes in real handy. 
While establishing the connection, you the -X option like below:
` ssh user@hostname -X `
And once you are connected, you can run a command like:

>xclac - for  graphical calculator
>xclock - for a clock
>gedit - for a text editor

In order for this to work, your server must have X11 forwarding enabled, which I will cover in the next coming section. Your client machine must have X11 server running. If you are on Linux, you are good. If you are on Mac, you would need something like **XQuartz**. If you are on Windows, you would need something like **Cygwin** or **Xming**.I personally don't use this feature at all. But its good to know.

###### - Connection and Execution of Command(s)
If you want to just execute a quick command or commands but dont want to have a shell session open, you can use the following command:
> ssh user@hostname [command]
ssh user@hostname uptime
>
By using double quotes and semicolon you can execute multiple commands. Here is an example:
> ssh user@hostname "uptime; ls -l; pwd; whoami; hostname"

###### - Connection with a Key  
Usually we generate a key pair, keep the private key with us and the public key on our server. However sometimes, eg: AWS, when you want to connect to your ec2 instance, you need to provide your private key to establish the connection.
So in that case, you can use the following command:
` ssh user@hostname -i path_to_your_private_key `
 
 The easier way is to add the key to your ssh agent.
 First you need to make sure the ssh agent is running on your machine.
`ssh-agent`
If its not running, you can start it by: `eval "$(ssh-agent -s)"`
Then you can add your key to the ssh agent by running: `ssh-add path_to_your_private_key`

