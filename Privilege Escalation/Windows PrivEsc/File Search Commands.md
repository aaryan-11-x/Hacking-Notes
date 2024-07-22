1) Find a File which contains [keyword] in it's **Name** :-
```powershell
findstr /si password *.txt *.ini *.xml *.config *.cfg
findstr /sim password *.txt *.ini *.xml *.config *.cfg
findstr /spin "password" *.*
Get-ChildItem -Path C:\Users\dave\ -Include *.txt,*.ini,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path "C:\" -Filter ".git" -Recurse -Force

# /si: Search in current and subdirectories, ignore cases
# /n: Display line numbers too
# Search for string "password" in .txt files
```

2) Search for a filename :-
```cmd
where /r c: [keyword]
```

3) List all files in **directory tree**
```cmd
cd/
dir /b/s *.txt
```

4) List *hidden* files
```cmd
dir /a:h-d /b/s
```

5) List all **System Files**
```cmd
dir /a:s-d /b/s
```

6) List all **ReadOnly Files**
```cmd
dir /a:r-d /b/s
```

%% If you remove the -d from all commands above it will list directories too. %%

---
# Powershell
```powershell
# Show hidden files in a folder
dir -Force

# Recursive ls with hidden items
Get-ChildItem -Recurse
Get-ChildItem -Recurse -Force

# Recursive ls with hidden items (Only Directories)
Get-ChildItem -Recurse -Directory -Force

# Find a file
Get-ChildItem -Path "C:\Users" -Filter "local.txt" -Recurse
```

