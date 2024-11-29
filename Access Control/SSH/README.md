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

- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) ([GitHub](https://github.com/github/putty))

  PuTTY is a free and open-source terminal emulator, serial console and network file transfer application. It supports several network protocols, including SCP, SSH, Telnet, rlogin, and raw socket connection. It can also connect to a serial port. The name "PuTTY" has no official meaning.

  - `scoop install putty`

- [Xshell](https://www.netsarang.com/en/xshell/)

[SSH implementation comparison](https://ssh-comparison.quendi.de/)

[Comparison of SSH servers - Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_SSH_servers)

[Comparison of SSH clients - Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_SSH_clients)

## Authentication
[How to automate SSH login with password? - Server Fault](https://serverfault.com/questions/241588/how-to-automate-ssh-login-with-password)
- PuTTY
  - `plink -pw`
  - `pscp -pw` / `pscp -pwfile`

- `sshpass`

  > sshpass is broken by design. When the ssh server is not added already in my `known_hosts`, `sshpass` will not show me the message to add the server to my known hosts, `passh` do not have this problem.

- [passh: 𝐬𝐬𝐡𝐩𝐚𝐬𝐬 is 𝒃𝒓𝒐𝒌𝒆𝒏 by design](https://github.com/clarkwang/passh)

[ssh-agent - Wikipedia](https://en.wikipedia.org/wiki/Ssh-agent)

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

Client:
```
ssh Administrator@1.2.3.4
```

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
$remotePowershell = "powershell Add-Content -Force -Path $env:ProgramData\ssh\administrators_authorized_keys -Value '''$authorizedKey''';icacls.exe ""$env:ProgramData\ssh\administrators_authorized_keys"" /inheritance:r /grant ""Administrators:F"" /grant ""SYSTEM:F"""

# Connect to your server and run the PowerShell using the $remotePowerShell variable
ssh username@domain1@contoso.com $remotePowershell
```
[解决git生成ssh密钥失败问题，本机用户名中文乱码导致密钥生成失败。\_enter file in which to save the key - CSDN博客](https://blog.csdn.net/qq_43689668/article/details/114457483)
- `-C "Alice"`

Permissions:
- [Windows SSH server refuses key based authentication from client - Super User](https://superuser.com/questions/1445976/windows-ssh-server-refuses-key-based-authentication-from-client)
- [windows - Unable to use publickey authentication on Win32 Open SSH server - Super User](https://superuser.com/questions/1538449/unable-to-use-publickey-authentication-on-win32-open-ssh-server)

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

- `Start-Job`, `Start-Process` and `start` don't work.
  
  [How do you run a program over SSH and keep it running when you detach? : r/WindowsHelp](https://www.reddit.com/r/WindowsHelp/comments/pesdkd/how_do_you_run_a_program_over_ssh_and_keep_it/)
