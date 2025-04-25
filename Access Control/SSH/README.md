# Secure Shell
[Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)

```sh
ssh remote_username@host
```

## Implementations
- [OpenSSH](http://www.openssh.com/) ([GitHub](https://github.com/openssh/openssh-portable))
  - [Portable OpenSSH](https://github.com/PowerShell/openssh-portable) (formerly Windows for OpenSSH)
    - [Win32-OpenSSH: Win32 port of OpenSSH](https://github.com/powershell/Win32-OpenSSH)

  [Why does sshd requires an absolute path? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/109380/why-does-sshd-requires-an-absolute-path)

- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) (~~[GitHub](https://github.com/github/putty)~~)

  PuTTY is a free and open-source terminal emulator, serial console and network file transfer application. It supports several network protocols, including SCP, SSH, Telnet, rlogin, and raw socket connection. It can also connect to a serial port. The name "PuTTY" has no official meaning.

  - `scoop install putty`
  - SSH1, SSH2, TELNET, rlogin
  - `-pw`/`-pwfile`
  - SCP, SFTP
  - Port forwarding, SOCKS (no VPN)

- [wolfSSL/wolfssh: wolfSSH is a small, fast, portable SSH implementation, including support for SCP and SFTP.](https://github.com/wolfSSL/wolfssh)

- [Xshell](https://www.netsarang.com/en/xshell/)

Rust:
- [ssh2-rs: Rust bindings for libssh2](https://github.com/alexcrichton/ssh2-rs)
- [openssh-rust/openssh: Scriptable SSH through OpenSSH in Rust](https://github.com/openssh-rust/openssh)
  - > This library supports only password-less authentication schemes.
- [sshs: Terminal user interface for SSH](https://github.com/quantumsheep/sshs)

Python:
- [Paramiko: The leading native Python SSHv2 protocol library.](https://github.com/paramiko/paramiko)
  - [Fabric: Simple, Pythonic remote execution and deployment.](https://github.com/fabric/fabric) ([Homepage](https://www.fabfile.org/))
    - [Documentation](https://docs.fabfile.org/en/latest/)
    - [Authentication](https://docs.fabfile.org/en/latest/concepts/authentication.html)

    ```python
    from fabric import Connection
    result = Connection('web1.example.com').run('uname -s', hide=True)
    msg = "Ran {0.command!r} on {0.connection.host}, got stdout:\n{0.stdout}"
    print(msg.format(result))
    # Ran 'uname -s' on web1.example.com, got stdout:
    # Linux
    ```

[SSH implementation comparison](https://ssh-comparison.quendi.de/)

[Comparison of SSH servers - Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_SSH_servers)

[Comparison of SSH clients - Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_SSH_clients)

[Client-Software for managing multiple SSH-Connections? : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/18zd8pt/clientsoftware_for_managing_multiple/)

### Cluster
- In-house foreach

  ```bash
  for host in $(cat hosts.txt); do ssh "$host" "$command" >"output.$host"; done
  ```

  ```bash
  tmpdir=${TMPDIR:-/tmp}/pssh.$$
  mkdir -p $tmpdir
  count=0
  while IFS= read -r userhost; do
      ssh -n -o BatchMode=yes ${userhost} 'uname -a' > ${tmpdir}/${userhost} 2>&1 &
      count=`expr $count + 1`
  done < userhost.lst
  while [ $count -gt 0 ]; do
      wait $pids
      count=`expr $count - 1`
  done
  echo "Output for hosts are in $tmpdir"
  ```

  PowerShell:
  ```pwsh
  param(
      [Parameter(Mandatory=$true)]
      [string]$script
  )

  $servers = (uv run servers.py).Split(',')

  foreach ($server in $servers) {
      Write-Host ('-' * 160)
      Write-Host "Server: $server"
      if (Test-Path $script) {
          plink -pw (uv run pw.py $server) -no-antispoof -m $script root@$server
      } else {
          plink -pw (uv run pw.py $server) -no-antispoof root@$server $script
      }
      if (!$?) {
          Write-Host "Error executing script on $server" -ForegroundColor Red
          exit 1
      }
  }
  ```

- tmux

  > I used to use tmux to do this. Split the screen half a dozen times. Connect each pane to a separate instance. Enter `setw synchronize-panes on` and you are now running commands on 6 instances simultaneously. From there you can run `htop` to see resources being used on all instances on one monitor, run `apt-get` commands, etc.

  > It's not quite as feature complete as cluster ssh, however I am also using it and have bound it to "v" with this tmux config:
  > `bind-key v setw synchronize-panes`

- Perl: [clusterssh: Cluster SSH - Cluster Admin Via SSH](https://github.com/duncs/clusterssh)

  [Cluster SSH - Manage Multiple Linux Servers Simultaneously - Putorius](https://www.putorius.net/cluster-ssh.html) ([Hacker News](https://news.ycombinator.com/item?id=21382998))
  > I used clusterssh in the past and it is really good at sending commands to multiple machines. However for any real work, I would strongly recommend keeping the typing to a bare minimum and do all your work inside a well tested script. Better yet, use ansible or something like it to manage multiple servers

  > There is plethora of similar tools (cssh, pssh, dsh) but Ansible ad-hoc mode superseeds these ones at any real task involving "simultaneous" management of Linux boxes.

- Ansible

  [Introduction to ad hoc commands --- Ansible Community Documentation](https://docs.ansible.com/ansible/latest/command_guide/intro_adhoc.html)

  > I don't have the hours and days to learn Ansible, and it doesn't make sense for such a small environment. And it's Yet Another Service my successor will have to learn in what is already an incredibly niche position.

- [MultiSSH](https://multissh.dev/)
  - Panels
- Rust
  - [csshw: Cluster SSH tool for Windows inspired by csshX](https://github.com/whme/csshw)
    - Panels
- C++
  - [chaos/pdsh: A high performance, parallel remote shell utility](https://github.com/chaos/pdsh)
- Python
  - [pssh: Parallel ssh](https://github.com/robinbowes/pssh) (discontinued)
- PuTTY

  [ssh - How to run one command to multiple servers with Plink in batch - Stack Overflow](https://stackoverflow.com/questions/68528328/how-to-run-one-command-to-multiple-servers-with-plink-in-batch)

  [Help on running plink for multiple servers. | PowerCLI](https://community.broadcom.com/vmware-cloud-foundation/discussion/help-on-running-plink-for-multiple-servers)

[Client-Software for managing multiple SSH-Connections? : r/selfhosted](https://www.reddit.com/r/selfhosted/comments/18zd8pt/clientsoftware_for_managing_multiple/)

[scripting - Automatically run commands over SSH on many servers - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/19008/automatically-run-commands-over-ssh-on-many-servers)

## Authentication
[How to automate SSH login with password? - Server Fault](https://serverfault.com/questions/241588/how-to-automate-ssh-login-with-password)
- PuTTY
  - `plink -pw`
  - `pscp -pw` / `pscp -pwfile`

- `sshpass`
  - [xhcoding/sshpass-win32: Windows version of http://sourceforge.net/projects/sshpass/](https://github.com/xhcoding/sshpass-win32)

  > sshpass is broken by design. When the ssh server is not added already in my `known_hosts`, `sshpass` will not show me the message to add the server to my known hosts, `passh` do not have this problem.

- [passh: 𝐬𝐬𝐡𝐩𝐚𝐬𝐬 is 𝒃𝒓𝒐𝒌𝒆𝒏 by design](https://github.com/clarkwang/passh)

VS Code:
- [Add option to persist/remember SSH password - Issue #7895 - microsoft/vscode-remote-release](https://github.com/microsoft/vscode-remote-release/issues/7895)
- [hereafter/ssh-vault-vscode](https://github.com/hereafter/ssh-vault-vscode)
  - Removed from the marketplace?

[ssh-agent - Wikipedia](https://en.wikipedia.org/wiki/Ssh-agent)

### Prompt spoofing
[PuTTY vulnerability vuln-auth-prompt-spoofing](https://www.chiark.greenend.org.uk/~sgtatham/putty/wishlist/vuln-auth-prompt-spoofing.html)

PuTTY:
- `-batch`
- [`-no-antispoof`](https://the.earth.li/~sgtatham/putty/0.83/htmldoc/Chapter7.html#plink-option-antispoof)

## Host keys
To create SSH protocol version 2 keys, use the ssh-keygen program that comes with OpenSSH:
```sh
# ssh-keygen [-q] [-b bits] [-t dsa | ecdsa | ed25519 | rsa] [-N new_passphrase] [-C comment] [-f output_keyfile] [-m format]
ssh-keygen -t rsa -N '' -f /etc/ssh/ssh_host_rsa_key 
ssh-keygen -t dsa -N '' -f /etc/ssh/ssh_host_dsa_key
```

For the version 1 keys, use
```sh
ssh-keygen -t rsa1 -N '' -f /etc/ssh/ssh_host_key 
```

[How To Set up SSH Keys on a Linux / Unix System - nixCraft](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/)

```sh
ssh-copy-id -i $HOME/.ssh/id_rsa.pub remote_username@host
```

[Why am I still getting a password prompt with ssh with public key authentication? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/36540/why-am-i-still-getting-a-password-prompt-with-ssh-with-public-key-authentication)
- `-vvv`
  
  Why not show an error message by default...? Like `Server refused our key` in WinSCP.

## Windows
[OpenSSH for Windows overview | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview)
- scp, sftp

[Get started with OpenSSH for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui&pivots=windows-server-2025)
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```
- > Starting with Windows Server 2025, OpenSSH is now installed by default.
- `Get-WindowsCapability: 没有注册类` / `Couldn't add`
- Even `OpenSSH.Client` is not added, these files may still exist in `C:\Windows\System32\OpenSSH`:
  ```
  scp.exe
  sftp-server.exe
  ssh-add.exe
  ssh-agent.exe
  ssh-keygen.exe
  ssh-shellhost.exe
  sshd.exe
  ```
  But running them may result in the following error:
  ```cmd
  C:\WINDOWS\System32\OpenSSH\scp.exe: CreateProcessW failed error:2
  C:\WINDOWS\System32\OpenSSH\scp.exe: posix_spawn: No such file or directory
  ```
  Just copy a `ssh.exe` there.

  [windows - SSH and SCP failed with "CreateProcessW failed error:2 posix\_spawn: No such file or directory" - Stack Overflow](https://stackoverflow.com/questions/65059250/ssh-and-scp-failed-with-createprocessw-failed-error2-posix-spawn-no-such-file)

- vs `scoop install openssh`?
  - `C:\Windows\System32\OpenSSH` may take precedence and cause errors.
  - `scoop install openssh` doesn't include `scp`

```powershell
Start-Service sshd

Set-Service -Name sshd -StartupType 'Automatic'

if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```
The profile of `OpenSSH-Server-In-TCP` is `Private` only by default.

Client:
```
ssh Administrator@1.2.3.4
```
[Microsoft Accounts](https://github.com/Chaoses-Ib/Windows/blob/main/Kernel/Users/Microsoft.md) are not supported.
- [ssh login to Microsoft account linked Windows impossible - Microsoft Community](https://answers.microsoft.com/en-us/windows/forum/all/ssh-login-to-microsoft-account-linked-windows/69ea0428-5b06-401c-bb86-e45228709e0b)

[How to SSH into Windows 10 or 11?](https://gist.github.com/teocci/5a96568ab9bf93a592d7a1a237ebb6ea)

### Key-based authentication
[Key-based authentication in OpenSSH for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement)

Client:
```powershell
ssh-keygen -t ecdsa

# By default the ssh-agent service is disabled. Configure it to start automatically.
# Make sure you're running as an Administrator.
Get-Service ssh-agent | Set-Service -StartupType Automatic

# Start the service
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ecdsa

# Get the public key file generated previously on your client
$authorizedKey = Get-Content -Path $env:USERPROFILE\.ssh\id_ecdsa.pub

# Generate the PowerShell to be run remote that will copy the public key file generated previously on your client to the authorized_keys file on your server
# Standard user:
# $remotePowershell = "powershell New-Item -Force -ItemType Directory -Path $env:USERPROFILE\.ssh; Add-Content -Force -Path $env:USERPROFILE\.ssh\authorized_keys -Value '$authorizedKey'"
$remotePowershell = "powershell Add-Content -Force -Path $env:ProgramData\ssh\administrators_authorized_keys -Value '''$authorizedKey''';icacls.exe ""$env:ProgramData\ssh\administrators_authorized_keys"" /inheritance:r /grant ""Administrators:F"" /grant ""SYSTEM:F"""

# Connect to your server and run the PowerShell using the $remotePowerShell variable
ssh username@domain1@contoso.com $remotePowershell
```
Never worked...

[解决git生成ssh密钥失败问题，本机用户名中文乱码导致密钥生成失败。\_enter file in which to save the key - CSDN博客](https://blog.csdn.net/qq_43689668/article/details/114457483)
- `-C "Alice"`

Permissions:
- [Windows SSH server refuses key based authentication from client - Super User](https://superuser.com/questions/1445976/windows-ssh-server-refuses-key-based-authentication-from-client)
- [windows - Unable to use publickey authentication on Win32 Open SSH server - Super User](https://superuser.com/questions/1538449/unable-to-use-publickey-authentication-on-win32-open-ssh-server)
- [Win10自带的ssh客户端key权限设置 - 知乎](https://zhuanlan.zhihu.com/p/108445764)

[Windows SSH server refuses key based authentication from client - Super User](https://superuser.com/questions/1445976/windows-ssh-server-refuses-key-based-authentication-from-client)

Remember the passphrase: [Windows 10 SSH client: password-less access - Super User](https://superuser.com/questions/1433917/windows-10-ssh-client-password-less-access)

[OpenSSH Server configuration for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-server-configuration)

### Logs
[ssh - Starting OpenSSH server in Windows with debug messages enabled (-d) - Super User](https://superuser.com/questions/1635361/starting-openssh-server-in-windows-with-debug-messages-enabled-d)
- `C:\ProgramData\ssh\sshd_config`
  ```
  SyslogFacility LOCAL0
  LogLevel Debug3
  ```
  ```pwsh
  Restart-Service sshd
  ```
- But `logs` directory is still created...

### Detaching
- `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList $targetPath`
  - Doesn't show the terminal window

  [minecraft - Windows ssh - How to keep proccess running after Disconnect - Stack Overflow](https://stackoverflow.com/questions/48842823/windows-ssh-how-to-keep-proccess-running-after-disconnect)

  [powershell - Starting a background process on Windows through an SSH connection that doesn't get stopped when SSH disconnects - Stack Overflow](https://stackoverflow.com/questions/75488795/starting-a-background-process-on-windows-through-an-ssh-connection-that-doesnt)

- `conhost $targetPath`

- LNK

- `Start-Job`, `Start-Process`, `start` and Windows Terminal don't work.
  
  [How do you run a program over SSH and keep it running when you detach? : r/WindowsHelp](https://www.reddit.com/r/WindowsHelp/comments/pesdkd/how_do_you_run_a_program_over_ssh_and_keep_it/)
