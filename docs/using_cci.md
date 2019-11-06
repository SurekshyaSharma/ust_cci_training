# Using CumulusCI

CumulusCI sits on top off the Salesforce Command Line Interface (Salesforce CLI) and Github. It facilitates installing managed packages and data into your scratch orgs so you can get developing quickly upon scratch org creation. It also facilitates the creation of managed and un-managed packages. Furthermore, it ushers you into a better interactive project cycle for collaborating with others. 

## Initing CumulusCI

Navigate to the git repository you want to set up with CumulusCI. And enter the following command:

```bash
cci project init
```

Folow the init instructions and answer the questions. Tips:

1. Defaults are usually the latest and greatest so why not use them (unless you know better)

2. Is it a managed package? Probably not for internal usage. If you don't know your namespace then say No.

3. If you use EDA at all in reference to your project make sure to allow it to add the EDA package. You can add any repository or managed package you want later if need be.

Now add the newly inited changes to git

```git
git add add .
git commit -m "Initialized CumulusCI Configuration"
```

Connect GIT to CumulusCI

```bash
cci service connect github
```

Push up your CumulusCI init files to your master branch.

```git
git push
```

## Connecting your Dev Hub

Connect your Dev Hub org. Your Dev Hub determines you licensing and my be your production org. Dev Hub has to be enabled in the set up of any org that you want to connect to in this way. As far as I understand the dev hub will inform sfdx of how many scratch orgs you can have running at once.

```bash
sfdx force:auth:web:login --setdefaultdevhubusername --setalias my-hub-org
```

*The my-hub-org may be any descriptive name you want*

A browser tab should open and you will be prompted to log into the org you want to be your dev hub

## Viewing Your Orgs

First look at what scratch orgs you currently have running by typing:

```bash
cci org list
```

If you haven't set up any scratch orgs or persistent orgs it will look like this:

```
┌Scratch Orgs───────┬──────┬─────────┬─────────┐
│ Name    │ Default │ Days │ Expired │ Config  │
├─────────┼─────────┼──────┼─────────┼─────────┤
│ beta    │         │ 1    │ -       │ beta    │
├─────────┼─────────┼──────┼─────────┼─────────┤
│ dev     │         │ 7    │ -       │ dev     │
├─────────┼─────────┼──────┼─────────┼─────────┤
│ feature │         │ 1    │ -       │ feature │
├─────────┼─────────┼──────┼─────────┼─────────┤
│ release │         │ 1    │ -       │ release │
└─────────┴─────────┴──────┴─────────┴─────────┘


┌Persistent Orgs─┬──────────┐
│ Name │ Default │ Username │
└──────┴─────────┴──────────┘
```

## Hooking up some persistant orgs

You can hook up any org that you have access (staging, dev, production, packaging) to your CCI instance and it will keep track of them only for that project instance. This means that cci can uses these persistent org names to deploy tasks again. This can include deploying code directly into them.

It should be noted that ideally you would only connect a packaging org. This packaging org would be used to create managed and un-managed packages to install/deploy your code in a versioned state to all of you other orgs. If your company doesn't have a packaging org spun up you can acquire a developer org from salesforce and use that as you packaging org.

The packaging concept may break down sometimes with new projects or when projects get tightly coupled to a companies internal processes. In this case you can still deploy to any persistent org instance directly. You should still maintain some discipline with scratch org first development with the repository as the source of truth rather than pushing to an org and testing development.

Connect to a sandbox org this way:

```bash
cci org connect --sandbox <org_label>
```

* org label can be whatever you want: dev, staging, production, eda_dev, my_dev, etc *

Connect to a staging, packaging or production org this way:

```bash
cci org connect <org_label>
```

*org label can be whatever you want: dev, staging, production, eda_dev, my_dev, etc*

You will be asked to login in each case and allow CumulusCI to have full access to that org.

Once you have successfully attached a persistant org you will see it at the bottom of your org list. after entering *cci org ilst* into your command line.

```html
┌Persistent Orgs────────┬───────────────────────────┐
│ Name        │ Default │ Username                  │
├─────────────┼─────────┼───────────────────────────┤
│ classic_dev │         │ dahl3702@stthomas.edu.dev │
└─────────────┴─────────┴───────────────────────────┘
```

You can make any persistant org your default persistant org by entering the following:

```bash
cci org default <org_name>
```

*org name is the name of a persistant org in your persistant org list*

## Creating Scratch Orgs

Scratch orgs are were all you development should initiate. Creating a scratch org that you can spin up quickly and work on immediately is crucial. This may mean installing other projects from GIT or installing managed/un-managed packages. First lets just spin one up.

So, let's just spin one up. This first command will spin up a scratch org with nothing installed. Apparently, simply by getting info on the dev org or will spin one up. Again, none of your code will be in this org yet. In the future you will want to run what is called a flow in CumulusCI. A flow is a sequence of tasks that build your org up correctly with your code and data in place and ready to go.

```bash
cci org info dev
```

Next you can make sure that your org has translated all your code to be deployed into the org of your choice. When you run the following command it will evaluate all your code and make an upper level, non-tracked, src directory to deploy.

```bash
cci task run dx_convert_from
```

Now you can deploy your code into your new org using this command:

```bash
cci task run deploy --org dev
```

*dev can be set to any org name including persistant orgs to deploy code.*

You can open the new dev org at any time during the above commands by calling it into a browser with this command:

```bash
cci org browser dev
```

*Again, you can open any org (persistant or scratch) by switching the 'dev' to that orgs name*

## Deleting an org

To delete an active scratch org use the following command

```git
cci org scratch_delete <ORG_NAME>
```

Example:

```git
cci org scratch_delete dev
```

## Getting changes out of your scratch org

CumulusCI will track click changes in your code and allow you to pull them down into your repository. This will facilitate your repository being the source of truth and tracking all changes whether they are clicks or code.

### Using the list_changes method

First run this command to see what changes have happened in your org replacing <ORG_NAME> with the name of the org you are comparing. MOst likely your org name will be dev.

```git
cci task run list_changes --org <ORG_NAME>
```

The list of changes may include changes you are not interested in tracking in your repository. You can exclude items like this:

```git
cci task run list_changes --org <ORG_NAME> -o exclude "<Excluded_item>"
```

*Example excluding all profile changes*

```git
cci task run list_changes --org <ORG_NAME> -o exclude "Profile:"
```

Now that you have your list honed down you can actually pull the changes into your repository:

```git
cci task run retrieve_changes --org <ORG_NAME> -o exclude "<Excluded_item>"
```

### Using the dx_pull method

Another method to pull down changes into your repository is to utilize the .forceignore file located in your repository. This allows you to cherry pick what you would like CumulusCI to automatically ignore simply by listing these items in your the .forceignore file. You may sill want to run the list_changes command above to help you create your ignore list.

[Here is the Salesforce documentation on how to use .forceingore](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_exclude_source.htm)

Once you are ignoring all the files that you don't want to track you can use the following CumulusCI task to retrieve org changes:

```git
cci task run dx_pull --org <ORG_NAME>
```

As you might expect there is a converse task that pushes data:

```git
cci task run dx_push --org <ORG_NAME>
```

