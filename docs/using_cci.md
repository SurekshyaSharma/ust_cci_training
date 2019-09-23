# Using CumulusCI

CumulusCI sits on top off the Salesfoce Command Line Interface (Salesforce CLI) and facilitates installing managed packages and data into your scratch orgs so you can get developing quickly upon scratch org creation. It also facilitates the creation of managed and unmanaged packages. Furthurmore it ushers you into a better itereative project cycle for collaborating with others. You will be using a combination of Git, Salesforce CLI, and the useful Cumulus tasks and flows.

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

Connect your Dev Hub org. Your Dev Hub determines you lisencing and my be your production org. Dev Hub has to be enabled in the set up of any org that you want to connect to in this way. As far as I understand the dev hub will inform sfdx of how many scratch orgs you can have running at once.

```bash
sfdx force:auth:web:login --setdefaultdevhubusername --setalias my-hub-org
```

*The my-hub-org may be any descriptive name you want*

A browser tab should open and you will be prompted to log into the org you want to be your dev hub

## Creating a dev scratch org

First look at what scratch orgs you curently have runing by typing:

```bash
cci org list
```

If you haven't set up any scratch orgs or persistant orgs it will look like this:

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

You can hook up any org that you have access (staging, dev, production, packaging) to your CCI instance and it will keep track of them only for that project instance. This means that cci can uses these persitance org names to deploy tasks agains. This can include deploying code directly into them.

It should be noted that idealy you would only connect a packaging org. This packaging org would be used to create managed and unmanaged packages to intall/deploy your code in a versioned state to all of you other orgs. If your company doesn't have a packaging org spun up you can aquire a developer org from salesforce and use that as you packaging org.

The packaging concept may break down sometimes with new projects or when projects get tightly coupled to a companies interal processes. In this case you can still deploy to any persistant org instance direcltly. You should still maintian some dicipline with scratch org first developement with the repository as the source of trutch rather than pushing to an org and testing development.

Connect to a sandbox org this way:

```bash
cci org connect --sandbox <org_label>
```

*org label can be whatever you want: dev, staging, production, eda_dev, my_dev, etc*

Connect to a staging, packaging or production org this way:

```bash
cci org connect <org_label>
```

*org label can be whatever you want: dev, staging, production, eda_dev, my_dev, etc*

You will be asked to login in each case and allow CumulusCI to have full access to that org.

Once you have successfully attached a persistant org you will see it at the bottom of your org list. after entering *cci org ilst* into your command line.

```
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



