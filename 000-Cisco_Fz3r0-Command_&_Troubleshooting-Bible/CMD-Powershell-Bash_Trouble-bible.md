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

### Fz3r0 - PowerShell Ping Like a Sir

````ps
$Target = "10.17.147.147"
$Interval = 1
$HighLatency = 150

$Failures = @()
$HighLatencyEvents = @()
$AllRTT = @()

$Sent = 0
$Received = 0

$PingSender = New-Object System.Net.NetworkInformation.Ping

Write-Host "Pinging $Target with timestamp... Press Ctrl+C to stop.`n"

try {
    while ($true) {
        $Sent++
        $TimeStamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
        $Reply = $PingSender.Send($Target, 1000)

        if ($Reply.Status -eq "Success") {
            $Received++
            $RTT = $Reply.RoundtripTime
            $AllRTT += $RTT

            Write-Host "[$TimeStamp] Reply from $Target : time=$RTT ms"

            if ($RTT -ge $HighLatency) {
                $HighLatencyEvents += "$TimeStamp  -  Reply from $Target  -  time=$RTT ms"
            }
        }
        else {
            Write-Host "[$TimeStamp] Request timed out for $Target" -ForegroundColor Red
            $Failures += "$TimeStamp  -  Request timed out"
        }

        Start-Sleep -Seconds $Interval
    }
}
finally {
    $Lost = $Sent - $Received
    if ($Sent -gt 0) {
        $LossPercent = [Math]::Round(($Lost / $Sent) * 100, 2)
    } else {
        $LossPercent = 0
    }

    if ($AllRTT.Count -gt 0) {
        $Min = ($AllRTT | Measure-Object -Minimum).Minimum
        $Max = ($AllRTT | Measure-Object -Maximum).Maximum
        $Avg = [Math]::Round(($AllRTT | Measure-Object -Average).Average, 2)
    } else {
        $Min = $Max = $Avg = 0
    }

    Write-Host "`n==== PING STATISTICS FOR $Target ===="
    Write-Host "    Packets: Sent = $Sent, Received = $Received, Lost = $Lost ($LossPercent% loss)"
    Write-Host "Approximate round trip times in milli-seconds:"
    Write-Host "    Minimum = $Min ms, Maximum = $Max ms, Average = $Avg ms"

    Write-Host "`n--- Packet Loss / Timeouts (with timestamp) ---"
    if ($Failures.Count -eq 0) {
        Write-Host "No packet loss detected."
    }
    else {
        $Failures | ForEach-Object { Write-Host $_ }
    }

    Write-Host "`n--- High Latency Events (>$HighLatency ms) ---"
    if ($HighLatencyEvents.Count -eq 0) {
        Write-Host "No high latency detected."
    }
    else {
        $HighLatencyEvents | ForEach-Object { Write-Host $_ }
    }

    Write-Host "`nTest finished at: $(Get-Date -Format "yyyy-MM-dd HH:mm:ss")"
}

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

## Trace Route 

### Windows Powershell

````py
# Native PowerShell traceroute
Test-NetConnection google.com -TraceRoute

# Traceroute CMD style
tracert google.com

# Skip DNS resolution (faster)
tracert -d google.com

# Continuous loop every 60s (save + screen) - MUST BE EXECUTED AS .bat SCRIPT
$LogPath = "C:\Users\Fz3r0\Documents\Ping_Logs\trace_google.log"

Write-Host "Starting traceroute monitor... Press Ctrl+C to stop." -ForegroundColor Cyan
Add-Content $LogPath "`n===== NEW SESSION $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') ====="

while ($true) {
    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    Write-Host "`n[$timestamp] Running traceroute to google.com..." -ForegroundColor Yellow
    Add-Content $LogPath "`n[$timestamp] Running traceroute to google.com..."

    # Run traceroute (Tracert) and append output
    tracert -d google.com | Tee-Object -FilePath $LogPath -Append

    # Wait 60 seconds before next run
    Start-Sleep -Seconds 60
}

````

### Windows CMD

````py
# Basic route (resolves hostnames)
tracert google.com

# Numeric only (faster, skips DNS resolution)
tracert -d google.com

# Limit to 20 hops
tracert -h 20 google.com

# Timeout per hop in milliseconds (example: 1 second)
tracert -w 1000 google.com

# Continuous loop every 60s (save + screen) - MUST BE EXECUTED AS .bat SCRIPT
:loop
echo %date% %time% >> C:\Users\Fz3r0\Documents\Ping_Logs\trace_google.log
tracert -d google.com >> C:\Users\Fz3r0\Documents\Ping_Logs\trace_google.log
timeout /t 60 >nul
goto loop

````

### Linux/Apple Bash

````py
# Numeric only (no DNS resolution)
traceroute -n google.com

# ICMP traceroute (requires sudo)
sudo traceroute -I google.com

# TCP traceroute (useful for testing specific ports)
sudo traceroute -T -p 443 google.com

# Interactive continuous traceroute + ping
mtr google.com

# Continuous traceroute every 60s (loop + timestamp)
LOG=~/trace_google.log
while true; do
  echo "[$(date '+%F %T')] Running traceroute to google.com..." | tee -a "$LOG"
  traceroute -n google.com | tee -a "$LOG"
  sleep 60
done


````

## DNS

### Windows Powershell

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

### Linux/Apple Bash

````py
# Basic DNS query
dig google.com

# DNS by type
dig google.com A
dig google.com MX
dig google.com TXT
dig google.com NS
dig google.com SOA

# Using a specific DNS server (example: 8.8.8.8)
dig @8.8.8.8 google.com A

# Compact output (short format)
dig @8.8.8.8 google.com A +short

# Reverse lookup (PTR)
dig -x 8.8.8.8

# Query multiple record types (A, AAAA, MX, NS, TXT)
for type in A AAAA MX NS TXT; do
  echo "==== $type record ===="
  dig @8.8.8.8 google.com $type +noall +answer
done

# Query a specific authoritative nameserver directly
dig @ns1.example.com google.com A +noall +answer

# Trace DNS resolution path (root → authoritative)
dig +trace google.com

# DNSSEC-enabled query (checks for RRSIG/AD flag)
dig +dnssec google.com A

# Continuous every 10s (loop)
LOG=~/Documents/Ping_Logs/dns_probe.log
echo "===== NEW SESSION $(date '+%Y-%m-%d %H:%M:%S') =====" >> "$LOG"

while true; do
  TS=$(date '+%Y-%m-%d %H:%M:%S')
  echo -e "\n[$TS] Querying google.com (A)"
  echo "[$TS] Querying google.com (A)" >> "$LOG"

  dig @8.8.8.8 google.com A +noall +answer | tee -a "$LOG"
  sleep 10
done

````

## Port / Protocol Connections

### Windows Powershell

````py

# Automatic Port Connection by Fz3r0
$TargetHost = "google.com"
$TargetPort = 443
$Interval   = 10   # seconds between tests
$LogPath    = "C:\Users\Fz3r0\Documents\Ping_Logs\https-test.log"

Write-Host "Starting TCP connectivity test to $TargetHost on port $TargetPort..." -ForegroundColor Cyan
Add-Content $LogPath "`n===== NEW SESSION $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') - Testing ${TargetHost}:${TargetPort} ====="

while ($true) {
    $timestamp = Get-Date -Format 'yyyy-MM-dd HH:mm:ss'
    $sw = [System.Diagnostics.Stopwatch]::StartNew()
    try {
        $client = New-Object System.Net.Sockets.TcpClient
        $client.ReceiveTimeout = 10000
        $client.SendTimeout = 10000
        $client.Connect($TargetHost, $TargetPort)
        $sw.Stop()
        "$timestamp OK - Connected to ${TargetHost}:${TargetPort} in {0} ms" -f $sw.ElapsedMilliseconds |
            Tee-Object -FilePath $LogPath -Append
        $client.Close()
    }
    catch {
        $sw.Stop()
        "$timestamp FAIL - Timeout or error after {0} ms : $($_.Exception.Message)" -f $sw.ElapsedMilliseconds |
            Tee-Object -FilePath $LogPath -Append
    }
    Start-Sleep -Seconds $Interval
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

