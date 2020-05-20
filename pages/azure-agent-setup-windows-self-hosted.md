# Setup Self-hosted Windows Azure Agent
[Microsoft documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-windows?view=azure-devops)

1. Download Agent
```powershell
#Create folder for agent
mkdir C:\agent-folder; 
cd C:\agent-folder
# TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
# Download url
$url = "https://vstsagentpackage.azureedge.net/agent/2.168.2/vsts-agent-win-x64-2.168.2.zip"
# Output file name
$output = "vsts-agent-win-x64-2.168.2.zip"
Invoke-WebRequest -Uri $url -OutFile $output
#Unzip
Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory(".\$output", "$PWD")
#Delete zip
del ".\$output"
```

2. Configure the agent
```powershell
.\config.cmd --unattended --url https://dev.azure.com/{workspace} --auth pat --token xxxxxxxxxxxxx --pool PoolName --agent PoolName.AgentName --runAsService --windowsLogonAccount Domain\User --windowsLogonPassword password 
```

3. Remove configuration (incase you need to reconfigure)
```powershell
.\config.cmd remove
```
