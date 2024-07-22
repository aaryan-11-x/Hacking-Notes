1) Delete The *WEAKID* From Storage In Dev Tools. This Is Because We Want To Generate New *WEAKID's* For Analysis.

2) Fire Up [[Website Hacking/Tools/Burp Suite/Basics]] , Capture a Session, Generate a **Response** For The *Request* Send The *Request* to <mark style="background: #FFB86CA6;">Sequencer</mark>

![[Pasted image 20230401115753.png]]


3) Start **Live Capture** (Upto *4000 Requests* Are Enough), Analyze The Tokens

![[Pasted image 20230401122546.png]]

![[Pasted image 20230401122729.png]]

![[Pasted image 20230401122917.png]]



4) **Copy** All The *Captured Tokens* In a Text File, Sort Them, Look For Anamolies

Here, <mark style="background: #FFB8EBA6;">03 Is Not Captured Which Might Indicate that It's a Valid User</mark>
<mark style="background: #FFF3A3A6;">This Might Be A Timestamp As the Numbers Are Very Random</mark>
![[Pasted image 20230401123236.png]]


5) Capture another session With *WEAKID* & Send To Intruder

6) Change The *WEAKID* : 205**03**16803272401$03$                             %%    $ To set Payload Position    %%

7) Here Payload Is a Number From $04-05$ Incrementing By 1, Change The Payload Settings & Start The Attack

8) Look For Any Changes In Length, That Might Be a Valid User WEAK ID![[Pasted image 20230401124137.png]]

9) Change Your *WEAKID* :  2050316803272401$04$
![[Pasted image 20230401124327.png]]
