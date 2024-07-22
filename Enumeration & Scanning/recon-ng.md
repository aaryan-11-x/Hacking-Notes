### Creating a Workspace
Run `workspaces create WORKSPACE_NAME` to create a new workspace for your investigation. For example, `workspaces create thmredteam` will create a workspace named `thmredteam`.
`recon-ng -w WORKSPACE_NAME` starts recon-ng with the specific workspace.


### Seeding the Database
In reconnaissance, you are starting with *one piece of information and transforming it into new pieces of information*. For instance, you might start your research with a company name and use that to discover the domain name(s), contacts and profiles.

We want to insert the domain name `thmredteam.com` into the domains table. We can do this using the command `db insert domains`.
If we want to check the names of the tables in our database, we can run `db schema`.

```sh
pentester@TryHackMe$ recon-ng -w thmredteam 
[...]
[recon-ng][thmredteam] > db insert domains
domain (TEXT): thmredteam.com
notes (TEXT):
[*] 1 rows affected. 
[recon-ng][thmredteam] > marketplace search
```


### Recon-ng Marketplace
Before you install modules using the marketplace, these are some useful commands related to marketplace usage:

-   `marketplace search KEYWORD` to search for available modules with _keyword_.
-   `marketplace info MODULE` to provide information about the module in question.
-   `marketplace install MODULE` to install the specified module into Recon-ng.
-   `marketplace remove MODULE` to uninstall the specified module.

Run `marketplace search` to get a list of all available modules.
```sh
[recon-ng][thmredteam] > marketplace search domains- 

[*] Searching module index for 'domains-'
...
..
.
 D = Has dependencies. See info for details. K = Requires keys. See info for details.
```
We notice many subcategories under `recon`, such as `domains-companies`, `domains-contacts`, and `domains-hosts`. This naming tells us what kind of new information we will get from that transformation. For instance, `domains-hosts` means that the module will find hosts related to the provided domain.

Some modules, like `whoxy_whois`, require a key, as we can tell from the `*` under the `K` column.
Other modules have dependencies, indicated by a `*` under the `D` column. Dependencies show that *third-party Python libraries* might be necessary to use the related module.


### Working with Installed Modules
-   `modules search` to get a list of all the installed modules
-   `modules load MODULE` to load a specific module to memory
-   `options list` to list the options that we can set for the loaded module.
-   `options set <option> <value>` to set the value of the option.

**Example**
We have installed the module `google_site_web`, so let’s load it using `load google_site_web` and run it with `run`. We have already added the domain `thmredteam.com` to the database, so when the module is run, it will read that value from the database.

```sh
[recon-ng][thmredteam] > load google_site_web 
[recon-ng][thmredteam][google_site_web] > run
```


### Keys
Some modules *cannot be used without a key* for the respective service API. `K` indicates that you need to provide the relevant service key to use the module in question.

-   `keys list` lists the keys
-   `keys add KEY_NAME KEY_VALUE` adds a key
-   `keys remove KEY_NAME` removes a key

Once you have the set of modules installed, you can proceed to load and run them.
-   `modules load MODULE` loads an installed module
-   `CTRL + C` unloads the module.
-   `info` to review the loaded module’s info.
-   `options list` lists available options for the chosen module.
-   `options set NAME VALUE`
-   `run` to execute the loaded module.