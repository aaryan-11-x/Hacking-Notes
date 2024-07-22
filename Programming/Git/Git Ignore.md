# Git Ignore
When sharing your code with others, there are often files or parts of your project, you *do not want to share*.

<u>Examples</u>
-   log files
-   temporary files
-   hidden files
-   personal files

Git can specify which files or parts of your project should be ignored by Git using a `.gitignore` file.
Git will not track files and folders specified in `.gitignore`. However, the `.gitignore` file itself **IS** tracked by Git.

## Create .gitignore

To create a `.gitignore` file, go to the root of your local Git, and create it

```sh
touch .gitignore     # Linux
nul > .gitignore     # Windows
```

In `.gitignore`
```sh
# ignore ALL .log files  
*.log  
  
# ignore ALL files in ANY directory named temp  
temp/

# ignore ALL folders ending with "name"
*name/

# ignore ALL files that matches a single non-specific character   Ex: name1.file, names.file
name?.file

# ignore ALL files that matches a specified range
name[a-z].file

# ignore ALL files that matches a single character except the ones spesified in the set of characters
name[!abc].file
```

%% Note: In this case, we use a single `.gitignore` which applies to the entire repository. It is also possible to have additional `.gitignore` files in subdirectories. These only apply to files or folders within that directory %%
