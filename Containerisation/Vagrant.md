**Vagrant** is a tool that can create, configure and manage virtual machines or virtual machine environments. The VMs are not created and configured manually but are described in code in a `Vagrantfile`. To better structure the program code, the Vagrant file can include additional code files. The code can then be processed using the Vagrant CLI. In this way, we can create, provision, and start our own VMs. Moreover, if the VMs are no longer needed, they can be destroyed just as quickly and easily.

###### Installation
```sh
sudo apt install virtualbox virtualbox-dkms vagrant -y
```
```powershell
C:\> IEX((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
C:\> choco upgrade chocolatey
C:\> cinst virtualbox cyg-get vagrant
```
