## DNS

### Objectives

- An online nslookup tutorial can be found and should be read before doing the lab at **`https://www.thegeekstuff.com/2012/07/nslookup-examples/`**
- In this lab, you perform some packet sniffing on DNS packets and you use the command line utility **`nslookup`** to simulate recursive queries. This lab borrows ideas of labs from **`http://en.wikiversity.org`**
- Note that your computer must be connected to the Internet to do this lab.

### Task 1 - Using nslookup

Start a terminal window on the Pi.

```
nslookup google.ca
```

**1. What are the IP address listed for `google.ca`?**

```

```

```
nslookup -type=ns google.com
```

**2. What are the name servers listed for this domain name?**

```

```

```
nslookup -type=soa google.com
```

**3. What is the start of authority information listed for this zone?**

```

```

```
nslookup -type=mx google.com
```

**4. What are the mail exchangers listed for this domain name?**

```

```



### Task 2 - Domain algonquincollege.com

Redo Task 1 on domain **`algonquincollege.com`**

**5. What are the IP address listed for `algonquincollege.com`?**

```

```

**6. What are the name servers listed for this domain name?**

```

```

**7. What is the start of authority information listed for this zone?**

```

```

**8. What are the mail exchangers listed for this domain name?**

```

```



### Task 3 - Recursive query

Open a terminal window

```
nslookup algonquincollege.com
```

**9. What is the IP address listed?**

```

```

To simulate a recursive query:

```
nslookup -norecurse -type=ns com. a.root-servers.net
```

The **`-norecurse`** option forces **`nslookup`** to issue a non-recursive or iterative query. This is the type of query that DNS servers typically issue to other DNS servers.

Observe the results. Notice the name servers listed for the **`com`** domain.

**10. What is the first `com` name server IP address returned?**

```

```



```
nslookup -norecurse -type=ns algonquincollege.com <com. name server>
```

where **`<com. name server>`** is the **`com`** name server IP address listed .

Observe the results.

**11. What are the DNS servers listed for the `algonquincollege.com` domain.**

```

```

Select the first **`algonquincollege.com`** name server IP address returned.

```
nslookup -norecurse www.algonquincollege.com <algonquincollege.com. name server>
```

where **`<algonquincollege.com. name server>`** is the first **`algonquincollege.com`** name server IP address listed.

Observe the results.

**12. What is the IP returned?**

```

```

**13. Is it the same IP as the IP returned question 9?**

```

```



### Task 4- Packet sniffing

This step is to be done on your laptop.

1. Start **`Wireshark`**.
2. Start a new capture on your live interface card.
3. Start a wireshark capture by clicking on the shark fin icon.
4. Open a Terminal window.
5. Use **`nslookup`** to issue some dns queries.
6. Stop the capture by clicking the shark fin icon.
7. In filter box, type DNS to filter traffic to DNS query
8. Take some time to explore DNS traffic in the different panels.
9. **Make a screen shot of the packet capture DNS.jpg .**

Upload the screenshot and this file with your answers to GitHub.

