The [`subprocess`](https://docs.python.org/3/library/subprocess.html#module-subprocess "subprocess: Subprocess management.") module allows you to **spawn new processes, connect to their input/output/error pipes, and obtain their return codes**. It Tends To Replace Older Modules Like [os.system()] & [os.spawn]

1) To Open Up A Terminal :-
```python
subprocess.Popen(command, shell=True/False)
```
	- It's Recommended To Put Shell=False Cuz When You Put It As *True*, A New Shell Will Be *Forked* & Then You Need   to Follow All The Syntax & Schematics Of That Particular Shell Which Can Lead To A *Security Leak*
	- Also, If You Put Shell=False, You Need To Give Your Command In The Form Of A List

*When Shell Is False*
	![[Pasted image 20220630190049.png]]

*When Shell Is True*
	![[Pasted image 20220630184534.png]]

