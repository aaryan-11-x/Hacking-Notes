RainbowCrack is a general propose implementation of Philippe Oechslin's faster [time-memory trade-off](https://en.wikipedia.org/wiki/Space-time_tradeoff) technique. It crack hashes with [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table).

### rtgen
- **Syntax**
```
rtgen [hash_type]  [charset]  [plaintext_length_min]  [plaintext_length_min]  [table_index]  [chain_len]  [chain_num]  [part_index]
```

- *Character Sets* :-
![[Pasted image 20230102205809.png]]

- *Chains* :-
Uses <u>Reduction Function</u> To Select Some Values Of The Hash And **Generate A New Hash** Out Of It!
Reduces Time!
Choose [chain_len] && [chain_num] - **1000 (Optimum)**
Increase [chain_num] By 0's If You Aren't Able To Crack!

- *Table Index* :-
Put **0**

- *Part Index* :-
Splits The Rainbow Table.
Put **0** If You Want Just 1 Rainbow Table.


### rtcrack
First, You Have To **Sort** Your Rainbow Table!

Command :-
**rtsort** [path_name]

**NOTE : DON'T PUT FILE NAME, JUST PUT PATH NAME OF RAINBOW TABLE
IF PATH IS IN HOME, PUT "."

Then, Use **rcrack** To Start Cracking!

*Examples* :-
**rcrack** [path_name] **-h** 5d41402abc4b2a76b9719d911017c592
**rcrack** [path_name] **-l** hash.txt

- **-h** : Crack a *Single Hash*
- **-l** : Crack a *List Of Hashes*

![[Pasted image 20230102214824.png]]






