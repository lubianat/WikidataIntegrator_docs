# A deeper look into wdi_core.WDItemEngine()

The `WDItemEngine()` method of Wikidata Integrator (WDI) does a lot of stuff, and can be heavily customized.
In this docs, I'll take a look at each of the parameters, expanding on the [documentation](https://github.com/SuLab/WikidataIntegrator/blob/main/wikidataintegrator/wdi_core.py).

# Base parameters 

## wd_item_id

The item that will be updated on Wikidata. It is not strictly necessary, as WDI can infer the item from the `data` object that is passed. 

## new_item

A boolean that says whether a new item should be created (ignoring the inference of existing item, by the way).
When no item is identified from the data, `new_item` is set to True by wdi. 

## data

A list of one or more objects of the type `WDBaseDataType`. A `WDBaseDataType` which is WDI's implementation of property-value pairs for each of Wikidata's datatypes.
It is the 'juice', from which WDI know what to add and to which item to add. 

## append_value

A list of Wikidata properties that will have a different behaviour when the `.write()` method is called.
Instead of overwriting the information, WDI will append any new values and leave the pre-existing ones. 

It will still update references of existing statements that match the data, though.

# Fast run parameters

More on that mode in the [fast run tutorial](./advanced_fast_run.md). 

## fast_run

A boolean that sets the "fast run" mode.
The use of this parameter requires another one: `fast_run_base_filter`

## fast_run_base_filter

A property-value dictionary, setting a filter for the fast run mode.

## fast_run_use_refs
From documentation:
If `True`, fastrun mode will consider references in determining if a statement should
be updated and written to Wikidata. Otherwise, only the value and qualifiers are used. Default: False

# Custom references parameters

## global_ref_mode

Sets different Wikidata Integrator modes for handling references. Five modes exist:

- "KEEP_GOOD": The default mode; keeps good references and updates where needed.
- "STRICT_KEEP": Keeps all references as they are
- "STRICT_KEEP_APPEND": Keeps all references as they are and appends the new ones
- "STRICT_OVERWRITE": Overwrites all existing references
- "CUSTOM": For very fine-grained control, the user can provide a custom handling function via the `ref_handler` parameter

## ref_handler

The `ref_handler` parameter optionally takes a custom function for handling references. 
It is needed when `ref_mode` is set to "CUSTOM"

The function should take two argunments of the class `WDBaseDataType`:
  - The current statement on Wikidata
  - The statement that will be written to Wikidata
The function should return the WDItemEngine that will be written, where only the _references_ should be modified (but not properties or values).

This function is also used in fast run. 

## good_refs

Modified from  docs:

This parameter lets the user define blocks of good references. It is a list of dictionaries.
      
Each dictionary contains Wikidata properties as keys and the "good reference" values. If `None`, it
just checks for the existence of that property, regardless of value

Example: [{'P248': 'Q905695', 'P352': None, 'P407': None, 'P1476': None, 'P813': None}]
             
This example says that a good reference is the one that contains:
        
        - P248' (stated in) 'Q905695' (UniProt)
        - P352 (some Uniprot iD)
        - P407 (some title of the uniprot entry)
        - P1476 (some language of the work)
        - P813 (some date of retrieval. 
        
You can set as many blocks like this as you want, to specify a range of good reference sets. 

## keep_good_ref_statements

From docs:

Do not delete any statement which has a good reference, either defined in the
        good_refs list or by any other referencing mode.
