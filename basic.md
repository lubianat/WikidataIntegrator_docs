# Basic usage of Wikidata Integrator

The Wikidata Integrator (WDI) is a framework for writing Wikidata Bots. 

## Central functionalities: wdi_core and WDItemEngine 

Wikidata Integrator has a main submodule `wdi_core` (wdi stands for Wikidata Integrator, core stands for core). 

The module provides a set of functions to create and update Wikidata items. 

The main class in the module is `WDItemEngine` which does _a lot_ of stuff. 
If you provide `data` (see below what exactly `data` is), WDI will try and get a matching item from Wikidata.
If an item is found, it will be updated. If no item is found, the data will be used to create a new one. 

## Providing data to WDItemEngine

Wikidata supports many different data types, each of them represented in WDI inside the `wdi_core` submodule. 

For example, `WDString` is the class in WDI made for adding strings on Wikidata.
Let's say you want to add data for the item with [Entrez Gene ID](https://www.wikidata.org/wiki/Property:P351) = ["50943"](https://www.ncbi.nlm.nih.gov/gene/50943).

You'll first write code to create an object of the class `WDString` saying something like 
'the Wikidata item "x" should have property "P351" and the value "50943". 

```python3
from wikidataintegrator import wdi_core
        
entrez_value = "50943"
entrez_property = "P351"
     
entrez_statement = wdi_core.WDString(value=entrez_value,
                                   prop_nr=entrez_property)
```

Now that we have the `WDString` object, we will add it to a list. 
That is a default behaviour of WDI: you'll likely want to add multiple statements at the same time.
We are keeping it simple for now, and having a list of size 1. 

```python3
data = [entrez_statement]
```

With the data list set, we can create our WDItemEngine object:


```python3
wd_item = wdi_core.WDItemEngine(data=data)
```

This object contains the set of statements in the `data` object. 
Even though you just added statements about _something_, the WDItemEngine constructor
got from Wikidata the item that matched the data:  [FOXP3](https://www.wikidata.org/wiki/Q21163319).

You can see it by running:

```python3
print(wd_item.wd_item_id)
# "Q21163319"
```

If no object is found, the `wd_item.wd_item_id` will simply be empty. 

Before writing to Wikidata, we will have to login. 

## Login in to wikidata: wdi_login and WDLogin

Inside the submodule `wdi_login`, you'll find code to login to Wikidata in order to perform the actions. 
If you are writing a bot, you'll need a [Bot Account](https://www.wikidata.org/wiki/Wikidata:Bots). 
You can also use normal user credentials, but then you will have less permissions. 

The credentials are loaded in an object of the class `WDLogin` by the following code:

```python3
from wikidataintegrator import wdi_login
login_instance = wdi_login.WDLogin(user='<username>', pwd='<password>')
```

## Writing data to Wikidata: WDItemEngine.write

All objects of the class WDItemEngine have a method called `.write()` which writes data to Wikidata. 
To run it, just type:

```python3
 wd_item.write( login_instance)
 ```
Note that you are passing the login_instance as a parameter: the function will use the credentials there
to write to Wikidata.

If the `wd_item.wd_item_id` is empty, WDI will create a new item with the data.
If the `wd_item.wd_item_id` points to an Wikidata QID, WDI will add the statements to this item. 

As there is no new information, in this example, WDI will do nothing. 

## Adding labels, descriptions and aliases to items

Now let's try and write something new to Wikidata. 

We can change an items' label via WDI using the method `WDItemEngine.set_label`:

```python3
wd_item.set_label(label="FOXP3", lang="pt-br")
```

That line of code will add to the wd_item object the instructions for setting the label "FOXP3" in Brazilian portuguese.
Let's write it to Wikidata:

```python3
wd_item.write(login_instance)
```
WDI handle the whole bureaucracy of making the update; you can see the diff for this change [here](https://www.wikidata.org/w/index.php?title=Q21163319&diff=1420047243&oldid=1400447217&diffmode=source)

In the same way, we can add aliases and descriptions with `WDItemEngine.set_aliases` and `WDItemEngine.set_description`. 
The only detail is that the aliases should be provided as a `["list"]`, not a plain `"str"`.

```python3
wd_item.set_description(description="gene codificante da espécie Homo sapiens", lang="pt-br")
wd_item.set_aliases(aliases=["forkhead box P3"], lang="pt-br")
wd_item.write(login_instance)
```

Done! ([diff for that change on Wikidata](https://www.wikidata.org/w/index.php?title=Q21163319&diff=1420048883&oldid=1420047243&diffmode=source))










