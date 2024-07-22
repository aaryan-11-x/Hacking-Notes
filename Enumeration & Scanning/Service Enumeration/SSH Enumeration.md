```sh
ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" learner@192.168.50.52
```
While Doing ssh On A Target, We Might Get A *Banner Which Shows Us The Version Of ssh*. To Do That You Might Occur With A Situation Like This :-

![[Pasted image 20220713113050.png]]

- First It Asks For Key Exchange, Run The Command Highlighted In <mark style="background: #D2B3FFA6;">Purple</mark>
- Next, It'll Ask For A Cipher, Run The Command In Neon Blue.
- In This Case, We Didn't Get Anything But Sometimes We Might Get Something, So Keep Trying.

##### Common Errors & Fixes
```sh
# Unable to negotiate with 192.168.8.109 port 22: no matching host key type found. Their offer: ssh-dss
ssh -oHostKeyAlgorithms=+ssh-dss root@192.168.8.109

# Disable the Authenticity Prompt
ssh -o StrictHostKeyChecking=no 10.8.138.254

# sign_and_send_pubkey: no mutual signature supported
ssh martin@192.168.161.49 -i id_rsa -o PubkeyAcceptedKeyTypes=ssh-rsa
```

---
## Bypass Firewall
If `.bashrc` is modified to prevent logging you into **SSH**, you can execute commands:-
```sh
ssh user@192.168.1.1 "cat /etc/passwd"
```
