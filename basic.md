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
        
entrez_value = "50943
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
It does not yet points to any Wikidata item, though! It just has statements about _something_. 
That identification will be done in the writing step. 

Before writing to Wikidata, we will have to login. 

## Login in to wikidata: wdi_login and WDLogin

Inside the submodule `wdi_login`, you'll find code to login to Wikidata in order to perform the actions. 
If you are writing a bot, you'll need a [Bot Account](https://www.wikidata.org/wiki/Wikidata:Bots). 
You can also use normal user credentials, but then you will have less permissions. 

The credentials are loaded in an object of the class `WDLogin` by the following code:

`login_instance = wdi_login.WDLogin(user='<username>', pwd='<password>')`

## Writing data to Wikidata: WDItemEngine.write

All objects of the class WDItemEngine have a method called `.write()` which writes data to Wikidata. 
To run it, just type:

```python3
 wd_item.write( login_instance)
 ```
Note that you are passing the login_instance as a parameter: the function will use the credentials there
to write to Wikidata.
To perform the writing, WDI tries to identify an item with data in the `data` slot. In the example here, it 
would identify the item for [FOXP3](https://www.wikidata.org/wiki/Q21163319), which matches the data. 
It would not, however, write anything: that statement is already present! 

N











