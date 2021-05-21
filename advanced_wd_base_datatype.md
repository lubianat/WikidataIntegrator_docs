The `WDBaseDataType` class is implemented Inside the `wdi_core` script and is inherited by the classes for each specific datatype on Wikidata.

To understand its documentation, it is helpful to look at the documentation for [snaks](https://www.mediawiki.org/wiki/Wikibase/DataModel#Snaks),
the basic information structures to describe entities in Wikidata. 

In short, most snaks are PropertyValueSnaks, that  describe that an Entity has a certain Property with a given Value. 
The value should be consistent with what is accepted for the property in the Wikidata system 
(e.g. a property that is used for strings should only be used in snacks alongside values of type string.) 

There are also less common "PropertyNoValueSnaks" that explicit states something is absent (i.e. Angela Merkel (subject) has no children (property).)
and "PropertySomeValueSnak" to state that a value definitely exists, but is unknown (i.e. the date of death of some people born in the 17th century). 

## init

Modified from original: 

          value (str or int or tuple): Data value of the WD data snak
          snak_type (a str in ['value', 'novalue', 'somevalue']): The snak type of the WD data snak,
                depending if the value is a known (value), not existent (novalue) or unknown (somevalue).
          data_type (str): The WD data type declaration of this snak
          is_reference (bool) : States if the snak is a reference, mutually exclusive with qualifier
          is_qualifier (bool) : States if the snak is a qualifier, mutually exclusive with reference
          references (list) : A one level nested list with reference WD snaks of base type WDBaseDataType, e.g.
                references=[
                            [<WDBaseDataType>, <WDBaseDataType>],
                            [<WDBaseDataType>]
                            ]
                This will create two references, the first one with two statements, the second with one
          qualifiers (list) : A list with instances of WDBaseDataType or children of it, qualifiers for the WD mainsnak
          rank (a str in ['normal', 'deprecated', 'preferred']): The rank of a WD mainsnak, should determine the status of a value
          prop_nr (str): The WD property number a WD snak belongs to. A string with a prefixed 'P' and several digits e.g. 'P715' (Drugbank ID)

The constructor not only loads information, but it prepares the `self.json_representation` attribute that contains the JSON representation of this piece of information.
In the inital stage, it is very basic, with only boilerplate information:

```python3
        self.json_representation = {
            "snaktype": self.snak_type,
            "property": self.prop_nr,
            "datavalue": {},
            "datatype": self.data_type
        }
```

That attribute will be modified by the methods `set_value` and `get_json_representation` that will do the job to load with values and other stuff, respectively. 

In that context, "other stuff" means qualifiers, references and ranks. 

# Information setters

## Perform validations and set information to a slot
- set_references
- set_qualifiers
- set_rank
- set_id
- set_hash
- set_prop_nr

## Perform validations and set information to the JSON representation
- set_value: sets the 'datavalue' for the JSON in `self.json_representation`

# Acessors

For every `set_xxx` there is a `get_xxx`. 

There are also two methods, `is reference` and `is qualifier` that returns the "bool" parameters passed to the object.

# Other

## get_json_representation

Constructs the json representation extending the mainsnak with references, qualifiers and rank. 


