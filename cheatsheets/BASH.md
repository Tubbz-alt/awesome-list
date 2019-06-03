
## One Liners

### Block Bad IPs

Use iptables to block all bad ip addresses:

```
$ cat /var/log/maillog | grep 'lost connection after AUTH from unknown' | tail -n 5
May 10 11:19:49 srv4 postfix/smtpd[1486]: lost connection after AUTH from unknown[185.36.81.145]
May 10 11:21:41 srv4 postfix/smtpd[1762]: lost connection after AUTH from unknown[185.36.81.164]
May 10 11:21:56 srv4 postfix/smtpd[1762]: lost connection after AUTH from unknown[175.139.231.129]
May 10 11:23:51 srv4 postfix/smtpd[1838]: lost connection after AUTH from unknown[185.211.245.170]
May 10 11:24:02 srv4 postfix/smtpd[1838]: lost connection after AUTH from unknown[185.211.245.170]
```

Get the data to show only IPs:

```
cat /var/log/maillog | grep 'lost connection after AUTH from unknown' | cut -d'[' -f3 | cut -d ']' -f1 | tail -n5
185.36.81.164
175.139.231.129
185.211.245.170
185.211.245.170
185.36.81.173
```

Get the uniqe IP addresses:

```
$ cat /var/log/maillog | grep 'lost connection after AUTH from unknown' | cut -d'[' -f3 | cut -d ']' -f1 | sort | uniq
103.194.70.16
112.196.77.202
113.172.210.19
113.173.182.119
139.59.224.234
```

Redirect the output to iptables:

```
$ for ip in $(cat /var/log/maillog | grep 'lost connection after AUTH from unknown' | cut -d'[' -f3 | cut -d ']' -f1 | sort | uniq); do iptables -I INPUT -s ${ip} -p tcp --dport 25 -j DROP; done
```

## While Loops

### Run process for 5 Seconds

```
set -ex
count=0
echo "boot"
ping localhost &
while [ $count -le 5 ]
  do
    sleep 1
    count=$((count + 1))
    echo $count
  done
ps aux | grep ping
echo "tear down"
kill $!
sleep 2
```

## Redirecting Outputs

### Stdout, Stderr 

Redirect stderr to /dev/null:

```
grep -irl faker . 2>/dev/null
```

Redirect stdout to one file and stderr to another file:

```
grep -irl faker . > out 2>error
```

Redirect stderr to stdout (&1), and then redirect stdout to a file:

```
grep -irl faker . >out 2>&1
```

Redirect both to a file:

```
grep -irl faker . &> file.log
```


