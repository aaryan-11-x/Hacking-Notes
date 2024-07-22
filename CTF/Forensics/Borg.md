**Borg** is an open-source application that allows you to backup your data using *deduplicating*. This allows the application to create incremental backups. This means that only new files or files that have been modified are stored in the new backups.
This deduplicating system makes Borg the ideal tool to create backups daily since the size of the backups will increase according to the changes.

```sh
sudo apt install borgbackup
```

https://www.imaginelinux.com/backup-restore-files-using-borg-linux/

```sh
# Print all contents to the terminal
borg extract file::backup --stdout

# Exclude extracting files/directories on a specific depth
borg extract file::backup --strip-components 1
```