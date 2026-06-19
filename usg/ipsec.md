# ipsec

```bash
ipsec sha2 compatible enable
sa force-detection enable
# 配置本地和远端的网段
acl number 3000
 rule 5 permit ip source 192.168.xxx.0 0.0.0.255 destination 192.168.yyy.0 0.0.0.255

ipsec proposal name1
 encapsulation-mode auto
 esp authentication-algorithm sha2-256
 esp encryption-algorithm aes-256
#
ike proposal default
 encryption-algorithm aes-256 aes-192 aes-128
 dh group14
 authentication-algorithm sha2-512 sha2-384 sha2-256
 authentication-method pre-share
 integrity-algorithm hmac-sha2-256
 prf hmac-sha2-256
ike proposal name2
 encryption-algorithm aes-256
 dh group14
 authentication-algorithm sha2-256
 authentication-method pre-share
 integrity-algorithm hmac-sha2-256
 prf hmac-sha2-256
#
ike peer name3
 exchange-mode auto
 pre-shared-key 预共享密钥
 ike-proposal name2
 remote-id-type ip
 remote-id 对端IP
 local-id 本端IP
 dpd type periodic
 remote-address 对端IP
#
ipsec policy name4 name2 isakmp
 security acl 3000
 ike-peer name3
 proposal name1
 tunnel local applied-interface
 sa trigger-mode auto
 sa duration traffic-based 10485760
 sa duration time-based 3600
 route inject dynamic
#
```
