*Before starting to create the script you should :* 

**Define what you want to monitor**: Determine which metrics are important for you to monitor. For example, you might want to check CPU usage, memory usage, disk space, and the status of critical services like SSH, Apache, MySQL, etc.

**Research the commands**: Look for the commands you can use to gather the metrics you're interested in. For example:

- `top`, `uptime`, or `mpstat` for CPU usage.
- `free` or `vmstat` for memory usage.
- `df` or `du` for disk space.
- `systemctl status <service>` for service status.

**Test each command individually**: Before incorporating a command into your script, test it in your terminal to make sure it gives you the desired output.

**Incorporate error handling**: It's important to include error handling in your script to deal with situations where a command fails to execute or returns unexpected output.

**Decide on the output format**: Determine how you want to present the monitoring data. You might choose to display it in the terminal, write it to a log file, or send it via email.

**Add scheduling if needed**: If you want your script to run periodically, you can use tools like `cron` to schedule its execution at regular intervals.

**Test your script**: Once you've written your script, test it thoroughly to ensure it provides accurate monitoring data and behaves as expected in different scenarios.

**Refine and improve**: As you gain more experience and encounter different monitoring needs, you can refine and improve your script to make it more robust and efficient.

---

In my case, i want to monitor the service i put in place on my server : 

- `SSH`
- `DHCP`
- `DNS`
- `HTTP`

and of course the metrics of my server : 

- `Memory`
- `CPU`
- `Disk Usage`
- `Network`

So for it i will research the command needed for each metrics and service. 

Time and date : 
`echo "## Server Monitoring Report - $(date +'%F %k:%M:%S %Z')"`

Device info:
`hostnamectl`

Service running:
`sudo systemctl list-units --type=service | grep "running" | sed -e "s/loaded.*running.*/running/g" -e "s/.service//g"`

Connectivity:
`ping -c 1 google.com &> /dev/null && echo "status: connected" || echo "  Status: Disconnected"`

IP address:
`echo "  Public IP: $(curl -s ipecho.net/plain;echo)"`
`echo "  Private IP: $(/sbin/ip -o -4 addr list eth0 | awk "{print $4}" | cut -d/ -f1)"`

DNS:
`cat /etc/resolv.conf | grep nameserver | awk "{print $2}"`

Network services: 
`sudo ss -tlnp | tail -n+2 | tr -s " " | cut -d " " -f 1,4,7 | column -ts " "`

Memory usage:

`egrep --color 'Mem|Cache|Swap' /proc/meminfo`

Disk usage:

`df -h`

Disk I/O

`iostat -d`

Uptime:

`uptime`


