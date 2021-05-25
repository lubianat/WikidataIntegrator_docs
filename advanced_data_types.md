There are a number of different datatypes that can be used in Wikidata Integrator, mirroring the datatypes available on Wikidata.

What do we mean by a datatype? Well, items on Wikidata are attached to different types of values: free text, numbers, others items and so on. 

The datatypes of Wikidata Integrator extend `WDBaseDataType` for handling these different types of values, and building PropertyValueSnaks 
(see [this tutorial](advanced_wd_base_datatype.md) for  a primer on snaks and `WDBaseDataType`. 

Let's look at the different types and their Wikidata Integrator implementations:

# Main data types

## string (class `WDString`)

Used for properties that receive free text as values. 

## external-id (class `WDExternalID`)

Used for properties that receive text in a specified format, matching a database identifier pattern, as values. 

## wikibase-item (class `WDItem`)
Used for properties that receive Wikidata items as values. 

        :type value: str with a 'Q' prefix, followed by several digits or only the digits without the 'Q' prefix
        
## time (class `WDTime`)
Used for properties that receive dates and/or times as values. 
        :param time: A time representation string in the following format: '+%Y-%m-%dT%H:%M:%SZ'
        :type time: str in the format '+%Y-%m-%dT%H:%M:%SZ', e.g. '+2001-12-31T12:01:13Z'
        
# url (class `WDUrl`)

Used for properties that receive URL strings (e.g. "https://www.example.com")

# quantity (class `WDQuantity`)
Used for properties that receive quantities as values. May specify upper and lower bound. Default is dimensionless, 
but there is a number of [units that can be specified](https://www.wikidata.org/wiki/Wikidata:Units).

        :param value: The quantity value
        :type value: float, str
        :param upper_bound: Upper bound of the value if it exists, e.g. for standard deviations
        :type upper_bound: float, str
        :param lower_bound: Lower bound of the value if it exists, e.g. for standard deviations
        :type lower_bound: float, str
        :param unit: The WD unit item URL or the QID a certain quantity has been measured
                        in (https://www.wikidata.org/wiki/Wikidata:Units). The default is dimensionless, represented by
                        a '1'
        :type unit: str

## globe-coordinate (class `WDGlobeCoordinate`)
 Used for properties that take a coordinate on a globe (usually on [Earth](https://www.wikidata.org/wiki/Q2)) as a value. 
 You may specify other globes, like [Mars](https://www.wikidata.org/wiki/Q111), via the `globe` parameter.
 
        :param latitude: Latitute in decimal format
        :type latitude: float
        :param longitude: Longitude in decimal format
        :type longitude: float
        :param precision: Precision of the position measurement
        :type precision: float

# Data types for media files

## commonsMedia (class `WDCommonsMedia`)
Used for properties that take a media file as value, such as property for images.
These files are hosted on Wikimedia Commons, and only the filename (and not the image itself) is stored on Wikidata.
        :param value: The media file name from Wikimedia commons to be used as the value
        :type value: str

## localMedia (class `WDLocalMedia`)
    
    """
    Implements the data type for Wikibase local media files.
    The new data type is introduced via the LocalMedia extension
    https://github.com/ProfessionalWiki/WikibaseLocalMedia
    """
            
## geo-shape (class `WDGeoShape`)
 Used for properties that take a shape (on a map) as a value. For example, the 
 [geoshape](https://www.wikidata.org/wiki/Property:P3896) property links [New York City](https://www.wikidata.org/wiki/Q60)
 to its [shape in the .map format](https://commons.wikimedia.org/wiki/Data:Neighbourhoods/New_York_City.map) hosted in Wikimedia Commons. 

        :param value: The GeoShape map file name in Wikimedia Commons to be linked
        :type value: str
        
## tabular-data (class `WDGeoShape`)
  Used for properties that take as value tabular data stored on Wikimedia Commons, like [tabular case data](https://www.wikidata.org/wiki/Property:P8204)

# Datatypes for lexicographical information

## wikibase-lexeme (class `WDLexeme`)

Used for properties that take [lexemes](https://www.wikidata.org/wiki/Wikidata:Lexicographical_data/Documentation),  as values. 
A lexeme is a "lexical element of a language, such as a word, a phrase, or a prefix".

## wikibase-form (class `WDForm`):

Used for properties that take [forms](https://www.wikidata.org/wiki/Wikidata:Lexicographical_data/Documentation),  as values. 
A form is "each relevant combination of grammatical features, such as 2nd person / singular / past tense.".

## wikibase-sense (class `WDSense`):

Used for properties that take [senses](https://www.wikidata.org/wiki/Wikidata:Lexicographical_data/Documentation),  as values. 
Senses describe the different meanings of a lexeme.

# Other data types

## math (class `WDMath`)

Used for properties that receive text that encodes mathematical notation as values. 

## musical-notation (class `WDMusicalNotation`)
Used for properties that receive text that encodes musical notation as values. 


## wikibase-property (class `WDProperty`):
Used for properties that receive another property as values (for example, to state that one is a [subproperty of](https://www.wikidata.org/wiki/Property:P1647) the other). 
        
        :type value: str with a 'P' prefix, followed by several digits or only the digits without the 'P' prefix

## monolingualtext (class `WDMonolingualText`)
Used for properties that receive free text in a particular language, and thus needs a `language` specifier.

        :param value: The language specific string to be used as the value
        :type value: str
        :param prop_nr: The WD item ID for this claim
        :type prop_nr: str with a 'P' prefix followed by digits
        :param language: Specifies the WD language the value belongs to
        :type language: str

       
 
