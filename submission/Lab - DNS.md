### Task 1 - Using nslookup

Start a terminal window on the Pi.

```
nslookup google.ca
```

**1. What are the IP address listed for `google.ca`?**

```
Server:		192.168.0.1
Address:	192.168.0.1#53

Non-authoritative answer:
Name:	google.ca
Address: 172.217.13.99
Name:	google.ca
Address: 2607:f8b0:4020:806::2003
```

```
nslookup -type=ns google.com
```

**2. What are the name servers listed for this domain name?**

```
ns1.google.com
ns2.google.com
ns3.google.com
ns4.google.com
```

```
nslookup -type=soa google.com
```

**3. What is the start of authority information listed for this zone?**

```
google.com
	origin = ns1.google.com
	mail addr = dns-admin.google.com
	serial = 521713658
	refresh = 900
	retry = 900
	expire = 1800
	minimum = 60
```

```
nslookup -type=mx google.com
```

**4. What are the mail exchangers listed for this domain name?**

```
mail exchanger = 10 smtp.google.com
```



### Task 2 - Domain algonquincollege.com

Redo Task 1 on domain **`algonquincollege.com`**

**5. What are the IP address listed for `algonquincollege.com`?**

```
Server:		192.168.0.1
Address:	192.168.0.1#53

Non-authoritative answer:
Name:	algonquincollege.com
Address: 54.86.119.60
```

**6. What are the name servers listed for this domain name?**

```
algonquincollege.com	nameserver = ns3.algonquincollege.com.
algonquincollege.com	nameserver = ns5.algonquincollege.com.
```

**7. What is the start of authority information listed for this zone?**

```
algonquincollege.com
	origin = ns5.algonquincollege.com
	mail addr = fwadm.algonquincollege.com
	serial = 2023032801
	refresh = 3600
	retry = 3600
	expire = 1209600
	minimum = 3600
```

**8. What are the mail exchangers listed for this domain name?**

```
mail exchanger = 10 algonquincollege-com.mail.protection.outlook.com
```



### Task 3 - Recursive query

Open a terminal window

```
nslookup algonquincollege.com
```

**9. What is the IP address listed?**

```
54.86.119.60
```

To simulate a recursive query:

```
nslookup -norecurse -type=ns com. a.root-servers.net
```

The **`-norecurse`** option forces **`nslookup`** to issue a non-recursive or iterative query. This is the type of query that DNS servers typically issue to other DNS servers.

Observe the results. Notice the name servers listed for the **`com`** domain.

**10. What is the first `com` name server IP address returned?**

```
e.gtld-servers.net	internet address = 192.12.94.30
```



```
nslookup -norecurse -type=ns algonquincollege.com com	nameserver = e.gtld-servers.net
```

where **`<com. name server>`** is the **`com`** name server IP address listed .

Observe the results.

**11. What are the DNS servers listed for the `algonquincollege.com` domain.**

```
Server:		e.gtld-servers.net
Address:	2001:502:1ca1::30#53

Non-authoritative answer:
*** Can't find algonquincollege.com: No answer

Authoritative answers can be found from:
algonquincollege.com	nameserver = ns3.algonquincollege.com.
algonquincollege.com	nameserver = ns5.algonquincollege.com.
algonquincollege.com	nameserver = ns1.d-zone.ca.
algonquincollege.com	nameserver = ns2.d-zone.ca.
ns3.algonquincollege.com	internet address = 205.211.50.11
ns5.algonquincollege.com	internet address = 205.211.50.8
```

Select the first **`algonquincollege.com`** name server IP address returned.

```
nslookup -norecurse www.algonquincollege.com ns3.algonquincollege.com
```

where **`<algonquincollege.com. name server>`** is the first **`algonquincollege.com`** name server IP address listed.

Observe the results.

**12. What is the IP returned?**

```
54.86.119.60
```

**13. Is it the same IP as the IP returned question 9?**

```
Yes it is.
```
