# dtcl
_Distributed Commandline Interface written in bash_

The **dtcl** utility executes commands on multiple servers, using ssh to make the connections. The servers are targeted serially. You run the **dtcl** utility on the "local" server and the commands specified in **dtcl** execute on the target servers.

You can also copy files to all target servers using the `-f` option.

The **dtcl** utility works best with a passwordless Secure Shell (SSH) between the local server and all target servers. You can run **dtcl** without passwordless ssh but then you are forced to enter the password for every server.  

### ssh setup

To simplify setup of the ssh connect you can use the `-k` option to fetch the hosts-keys from the target servers and append them to the known_hosts file using a redirect e.g. `>> ~/.ssh/known_hosts`.
After fetching the host-keys you can integrate your ssh-key to the target servers _authorized_keys_ filed using:<br> 
`dtcl -l tux -g serverlist -f ~/.ssh/id_rsa.pub -c "cat id_rsa.pub >> ~/.ssh/authorized_keys"`.<br> 
Note that, when you use this command, you will have to enter your password twice for each server, one time for the file copy and one time for the command.


## dtcl usage

`dtcl [options] [-c command]`

### dtcl options 	


| &nbsp; &nbsp; &nbsp; &nbsp; Option &nbsp; &nbsp; &nbsp; &nbsp; | Description |
| ------------------------- | --- |
| `-h hosts` | Specifies a comma separated list of hosts where the command is executed. Can be used in conjunction with `-g`. |
| `-g groupfile` | Specifies a file containing a list of servers where the command is executed. Either server names or IP addresses can be used in the file. |
| `-l userid` | Identifies the user ID for logging in to another server. The default ID is root.|
| `-c command` | Specifies the command(s) to be executed. Commands containing white space must be enclosed in quotes. | 
| `-f files` | Specifies a list of files to be transferred to the users home directory of the target servers. |
| `-d directory` | Specifies a target directory for the `-f` option. |
| `-p` | Ping: checks the ssh-connection to the target servers. |
| `-k` | Keyscan: gathers the ssh-hostkeys from the target servers. To append them to the known_hosts file use a redirect e.g. `>> ~/.ssh/known_hosts`. |
| `-t` | Lists the target servers. |

### Examples

- Pings one server using the user tux.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -l tux -h linux-1 -p`

- List all target servers in groupfile all_linux_servers

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -g all_linux_servers -t`

- Execute the date command with the default user _root_ on host linux-1

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -h linux-1 -c date`

- Execute the uptime command on servers linux-1 and linux-2 with the user tux

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -h linux-1,linux-2 -l tux -c uptime`

- Execute a command on all servers in groupfile all_linux_servers

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -l tux -g all_linux_servers -c "df -h | grep root"`

- Copy two files into the home directoy of user tux on two servers

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -l tux -h linux1,linux-2 -f myfile1,myfile2`

- Copy a file to the directory _/var/www/html_ for all servers in the goupfile _webservers_

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -l www -g webservers -f myfile1.html -d /var/www/html`

- Fetch the host keys from all servers in group all_linux_servers and appends them to the local users _known_hosts_ file. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`dtcl -l tux -g all_linux_servers -k >> ~/.ssh/known_hosts`
