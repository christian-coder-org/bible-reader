# Website DNS changes

DNS is hosted on gandi.net as is the domain name `christian-coder.org`,

## Videos on GitHub pages setup

Visit this excellent video for GitHub pages custom domain setup.

https://www.youtube.com/watch?v=QxTOGJORhMo

## `dig` command

```sh
# COMMAND
% dig www.christian-coder.org

# OUTPUT
; <<>> DiG 9.10.6 <<>> www.christian-coder.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19911
;; flags: qr rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;www.christian-coder.org.	IN	A

;; ANSWER SECTION:
www.christian-coder.org. 300	IN	CNAME	christian-coder-org.github.io.
christian-coder-org.github.io. 3600 IN	A	185.199.110.153
christian-coder-org.github.io. 3600 IN	A	185.199.108.153
christian-coder-org.github.io. 3600 IN	A	185.199.111.153
christian-coder-org.github.io. 3600 IN	A	185.199.109.153

;; Query time: 376 msec
;; SERVER: 192.168.1.1#53(192.168.1.1)
;; WHEN: Tue Jun 24 15:49:49 MDT 2025
;; MSG SIZE  rcvd: 159
```

## whatsmydns.net

https://www.whatsmydns.net/#A/christian-coder.org
