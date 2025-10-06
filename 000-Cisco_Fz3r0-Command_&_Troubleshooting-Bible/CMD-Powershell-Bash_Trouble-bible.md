# 🧠🏗️🌐 Cisco IOS: `Fz3r0 Troubleshooting Bible`

![My Video](https://user-images.githubusercontent.com/94720207/165892585-b830998d-d7c5-43b4-a3ad-f71a07b9077e.gif)

### 🐦 Twitter  : [@fz3r0_OPs](https://twitter.com/Fz3r0_OPs)
### 🐱 Github  : [Fz3r0](https://github.com/fz3r0) 

---
 
#### Keywords: `Cisco` 

---

<br>

# 📄 `Index`

### Device Location

- 

#  CMD, Powershell, Bash :: `Fz3r0 Troubleshooting Bible`






## Ping

### Windows CMD

````py
# Simple ping
ping google.com
ping 8.8.8.8

# Continuous ping
ping -t 8.8.8.8

# Ping a specific number of times
ping -n 20 google.com

# Continuous ping with log output (Ctrl + C to stop)
ping -t google.com >> C:\Users\Fz3r0\Documents\Ping_Logs\ping_google_log.txt

# Continuous ping with timestamp (CMD trick using for loop)
for /f "tokens=*" %a in ('ping -t 8.8.8.8') do @echo %time% %a

````

### Windows Powershell

````py
# Basic ping
ping google.com
ping 8.8.8.8

# Continuous ping with timestamp
ping -t google.com | ForEach-Object {"$(Get-Date -Format 'HH:mm:ss') $_"}

# Continuous ping + timestamp saved to a file
ping -t google.com | ForEach-Object {"$(Get-Date -Format 'HH:mm:ss') $_"} >> C:\Users\Fz3r0\Documents\Ping_Logs\ping_google.log

# continuous ping with timestamp, show on screen and append to log
ping -t google.com | ForEach-Object {"$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') $_"} | Tee-Object -FilePath "C:\Users\Fz3r0\Documents\Ping_Logs\ping_google.log" -Append

# EXACTLY 10 pings (CMD style)
ping -n 10 google.com

# EXACTLY 10 pings (PowerShell native)
Test-Connection -ComputerName 8.8.8.8 -Count 10 -IPv4 | Select-Object Address,Latency,Status

# Save log with dynamic filename (timestamp)
$date = Get-Date -Format "yyyyMMdd_HHmmss"
ping -t 8.8.8.8 | ForEach-Object {"$(Get-Date -Format 'HH:mm:ss') $_"} >> "C:\Users\Fz3r0\Documents\Ping_Logs\ping_$date.log"

# Extract only timeouts from a ping log
Select-String -Path "C:\Users\Fz3r0\Documents\Ping_Logs\ping_google.log" -Pattern "Request timed out"

# Quick console graph (green dot = OK, red x = timeout)
ping -t 8.8.8.8 | ForEach-Object {
    if ($_ -match "time=") {Write-Host "." -NoNewline -ForegroundColor Green}
    elseif ($_ -match "Request timed out") {Write-Host "x" -NoNewline -ForegroundColor Red}
}

# High TTL (up to 255). Useful for testing hop expiration.
ping -i 255 8.8.8.8

# Low TTL (to force “Time Exceeded” near the source)
ping -i 2 8.8.8.8

# Timeout (in milliseconds). Example: 2 seconds.
ping -w 2000 8.8.8.8

# Payload size (buffer length) for MTU testing
# Standard Ethernet non-fragmented = 1472 (1500 - 20 IP - 8 ICMP)
ping -f -l 1472 8.8.8.8

# Jumbo frame test (~9000 MTU = 8972 payload)
ping -f -l 8972 10.0.0.10

# Force IPv4 or IPv6
ping -4 google.com
ping -6 ipv6.google.com

# Resolve hostname from IP address
ping -a 8.8.8.8

# Specify source IP (useful on multihomed hosts)
ping -S 10.10.10.5 8.8.8.8

# Record route (IPv4 only, max 9 hops)
ping -r 9 8.8.8.8

# Quick traceroute without DNS resolution
tracert -d 8.8.8.8

# Pathping (latency + packet loss per hop)
pathping 8.8.8.8

# Custom ICMP message using .NET Ping class
$ping = New-Object System.Net.NetworkInformation.Ping
$data = [Text.Encoding]::ASCII.GetBytes("I am Fz3r0")
$result = $ping.Send("8.8.8.8", 1000, $data)
Write-Host "Reply from $($result.Address): time=$($result.RoundtripTime)ms status=$($result.Status)"

````

### Linux/Apple Bash

````py
# --- Basic ping ---
ping google.com
ping 8.8.8.8

# Continuous ping with timestamp
ping 8.8.8.8 | while read line; do echo "$(date '+%H:%M:%S') $line"; done

# Continuous ping + timestamp saved to a file
ping google.com | while read line; do echo "$(date '+%Y-%m-%d %H:%M:%S') $line"; done >> ~/Ping_Logs/ping_google.log

# Continuous ping with timestamp, show on screen and append to log
mkdir -p ~/Ping_Logs
ping google.com | while read line; do
  echo "$(date '+%Y-%m-%d %H:%M:%S') $line" | tee -a ~/Ping_Logs/ping_google.log
done

# EXACTLY 10 pings
ping -c 10 google.com

# Save log with dynamic filename (timestamp)
DATE=$(date +%Y%m%d_%H%M%S)
ping 8.8.8.8 | while read line; do
  echo "$(date '+%H:%M:%S') $line"
done >> ~/Ping_Logs/ping_$DATE.log

# Extract only timeouts from a ping log
grep "timeout" ~/Ping_Logs/ping_google.log

# Quick console graph (dot = ok, x = timeout)
ping 8.8.8.8 | while read line; do
  if [[ $line == *"time="* ]]; then
    echo -n "."
  elif [[ $line == *"timeout"* ]]; then
    echo -n "x"
  fi
done

# High TTL (up to 255). Useful for hop expiration testing.
ping -t 255 8.8.8.8

# Low TTL (to force “Time Exceeded” near the source)
ping -t 2 8.8.8.8

# Timeout per reply (seconds)
ping -W 2 8.8.8.8

# Total deadline for the command (stop after 20s)
ping -w 20 8.8.8.8

# Payload size for MTU testing
# Ethernet non-fragmented = 1472 (1500 - 20 IP - 8 ICMP)
ping -M do -s 1472 8.8.8.8

# Jumbo frame test (~9000 MTU → 8972 payload)
ping -M do -s 8972 10.0.0.10

# Force IPv4 or IPv6
ping -4 google.com
ping -6 ipv6.google.com

# Resolve hostname from IP (reverse lookup)
ping -c 1 -a 8.8.8.8

# Specify source IP (useful on multihomed hosts)
ping -I eth0 google.com

# Record route (IPv4 only, max 9 hops)
ping -R 8.8.8.8

# Quick traceroute without DNS resolution
traceroute -n 8.8.8.8

# Pathping equivalent (combine ping + mtr)
mtr -n 8.8.8.8

# Custom ICMP message with hping3 (must be root)
# -1 = ICMP mode, -E = file or string payload, -d = payload length
sudo hping3 -1 8.8.8.8 -E <(echo "I am Fz3r0") -d 32

````


## DNS

### Windows Powershell

````py
````py
# Basic DNS querie
nslookup google.com

# DNS by type
nslookup -type=A google.com 8.8.8.8
nslookup -type=MX google.com 8.8.8.8
nslookup -type=TXT google.com 8.8.8.8
nslookup -type=NS google.com 8.8.8.8

# Reverse lookup
nslookup 8.8.8.8

# Use a specific DNS server for resolution (e.g. 1.1.1.1 Cloudflare)
nslookup google.com 1.1.1.1

# Query specific nameserver
nslookup google.com ns1.example.com

# Continuous every 10s (loop)
$LogPath = "C:\Users\Fz3r0\Documents\Ping_Logs\dns_probe.log"

Write-Host "Starting DNS probe... Press Ctrl+C to stop." -ForegroundColor Cyan
Add-Content $LogPath "`n===== NEW SESSION $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') ====="

while ($true) {
    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    Write-Host "`n[$timestamp] Querying google.com (A)" -ForegroundColor Yellow

    Resolve-DnsName google.com -Type A -Server 8.8.8.8 |
        Tee-Object -FilePath $LogPath -Append

    Start-Sleep -Seconds 10
}

````

### Windows CMD

````py
# Basic DNS querie
nslookup google.com

# DNS by type
nslookup -type=A google.com 8.8.8.8
nslookup -type=MX google.com 8.8.8.8
nslookup -type=TXT google.com 8.8.8.8
nslookup -type=NS google.com 8.8.8.8

# Reverse lookup
nslookup 8.8.8.8

# Use a specific DNS server for resolution (e.g. 1.1.1.1 Cloudflare)
nslookup google.com 1.1.1.1

# Query specific nameserver
nslookup google.com ns1.example.com

# Continuous every 10s (loop) - MUST BE EXECUTED AS .bat SCRIPT
:loop
echo %date% %time% >> C:\Users\Fz3r0\Documents\Ping_Logs\dns_probe.log
nslookup -type=A google.com 8.8.8.8 >> C:\Users\Fz3r0\Documents\Ping_Logs\dns_probe.log
timeout /t 10 >nul
goto loop

# Fixed log path (folder already exists)
$LogPath = "C:\Users\Fz3r0\Documents\Ping_Logs\dns_probe.log"

Write-Host "Starting DNS probe... Press Ctrl+C to stop." -ForegroundColor Cyan
Add-Content $LogPath "`n===== NEW SESSION $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') ====="

# Continuous every 10s (loop) 
while ($true) {
    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    Write-Host "`n[$timestamp] Querying google.com (A)" -ForegroundColor Yellow

    # Run DNS query and show on screen + append to log
    Resolve-DnsName google.com -Type A -Server 8.8.8.8 |
        Tee-Object -FilePath $LogPath -Append

    # Wait 10 seconds
    Start-Sleep -Seconds 10
}

````






# 🗃️ Resources

- 

---

<span align="center"> <p align="center"> ![giphy](https://user-images.githubusercontent.com/94720207/166587250-292d9a9f-e590-4c25-a678-d457e2268e85.gif) </p> </span> 

&nbsp;

<span align="center"> <p align="center"> _I hope this information was useful for someone_ </p> </span> 
<span align="center"> <p align="center"> _and please, don't forget to enjoy your days..._ </p> </span> 
<span align="center"> <p align="center"> _...It is getting dark... so dark..._ </p> </span> 

&nbsp;

<span align="center"> <p align="center"> _In the mist of the night you could see me come, where shadows move and Demons lie..._ </p> </span> 
<span align="center"> <p align="center"> _I am [Fz3r0 💀](https://github.com/Fz3r0/) and the Sun no longer rises..._ </p> </span> 

---

---

> ![hecho en mexico 5](https://user-images.githubusercontent.com/94720207/166068790-fa1f243d-2db9-4810-a6e4-eb3c4ad23700.png)
>
> _- Hecho en México - by [Fz3r0 💀](https://github.com/Fz3r0/)_  
>
> _"In the mist of the night you could see me come, where shadows move and Demons lie..."_ 

