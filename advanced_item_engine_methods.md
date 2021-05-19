(IN PROGRESS)

The `WDItemEngine` class also holds a variety of methods to interact with Wikidiata. 

The core, `.write()` was introduced in the basic document.

In this tutorial we will take a look at a few of the other methods in the class.

There are a lot of methods in that class, and it can look overwhelming at first, so lets take it part by part.

# Writing and deleting data on Wikidata

## write   

    def write(self, login, bot_account=True, edit_summary='', entity_type='item', property_datatype='string',
              max_retries=1000, retry_after=60):
        """
        Writes the WD item Json to WD and after successful write, updates the object with new ids and hashes generated
        by WD. For new items, also returns the new QIDs.
        :param login: a instance of the class PBB_login which provides edit-cookies and edit-tokens
        :param bot_account: Tell the Wikidata API whether the script should be run as part of a bot account or not.
        :type bot_account: bool
        :param edit_summary: A short (max 250 characters) summary of the purpose of the edit. This will be displayed as
            the revision summary of the Wikidata item.
        :type edit_summary: str
        :param entity_type: Decides wether the object will become an item (default) or a property (with 'property')
        :type entity_type: str
        :param property_datatype: When payload_type is 'property' then this parameter set the datatype for the property
        :type property_datatype: str
        :param max_retries: If api request fails due to rate limiting, maxlag, or readonly mode, retry up to
        `max_retries` times
        :type max_retries: int
        :param retry_after: Number of seconds to wait before retrying request (see max_retries)
        :type retry_after: int
        :return: the WD QID on sucessful write
        """

### Parameters

- login
- bot_account
- edit_summary
- entity_type
- property_datatype
- max_retries
- retry_after

## update

```
    def update(self, data, append_value=None):
        """
        This method takes data, and modifies the Wikidata item. This works together with the data already provided via
        the constructor or if the constructor is being instantiated with search_only=True. In the latter case, this
        allows for checking the item data before deciding which new data should be written to the Wikidata item.
        The actual write to Wikidata only happens on calling of the write() method. If data has been provided already
        via the constructor, data provided via the update() method will be appended to these data.
        :param data: A list of Wikidata statment items inheriting from WDBaseDataType
        :type data: list
        :param append_value: list with Wikidata property strings where the values should only be appended,
            not overwritten.
        :type: list
        """
```

## delete_statement
    @staticmethod
    def delete_statement(statement_id, revision, login, mediawiki_api_url='https://www.wikidata.org/w/api.php',
                         user_agent=config['USER_AGENT_DEFAULT']):

# References and qualifiers

    @classmethod
    def count_references(self, prop_id, user_agent=config['USER_AGENT_DEFAULT']):

    @classmethod
    def get_reference_properties(self, prop_id, user_agent=config['USER_AGENT_DEFAULT']):


    @classmethod
    def get_qualifier_properties(self, prop_id, user_agent=config['USER_AGENT_DEFAULT']):



# Others

## get_rdf
    @staticmethod
    def get_rdf(qid, format="turtle", mediawiki_api_url=None):
        """
            :param qid: Wikidata identifier to extract the RDF of
            :format RDF from to return takes (turtle, ntriples, rdfxml, see https://rdflib.readthedocs.io/en/stable/apidocs/rdflib.html)
            :param mediawiki_api_url: default to wikidata's api, but can be changed to any wikibase
            :return:
        """

## merge_items

    @staticmethod
    def merge_items(from_id, to_id, login_obj, mediawiki_api_url=None,
                    ignore_conflicts='', user_agent=None):
        """
        A static method to merge two Wikidata items
        :param from_id: The QID which should be merged into another item
        :type from_id: string with 'Q' prefix
        :param to_id: The QID into which another item should be merged
        :type to_id: string with 'Q' prefix
        :param login_obj: The object containing the login credentials and cookies
        :type login_obj: instance of PBB_login.WDLogin
        :param mediawiki_api_url: The MediaWiki url which should be used
        :type mediawiki_api_url: str
        :param ignore_conflicts: A string with the values 'description', 'statement' or 'sitelink', separated
                by a pipe ('|') if using more than one of those.
        :type ignore_conflicts: str
        """

## `_init_ref_system`  

    @classmethod
    def _init_ref_system(cls, sparql_endpoint_url=None):
        db_query = '''
        SELECT DISTINCT ?db ?wd_prop WHERE {
            {?db wdt:P31 wd:Q2881060 . } UNION
            {?db wdt:P31 wd:Q4117139 . } UNION
            {?db wdt:P31 wd:Q8513 . } UNION
            {?db wdt:P31 wd:Q324254 .}
            OPTIONAL {
              ?db wdt:P1687 ?wd_prop .
            }
        }'''

## delete_item

    @staticmethod
    def delete_item(item, reason, login, mediawiki_api_url=None, user_agent=None):
        """
        Takes a list of items and posts them for deletion by Wikidata moderators, appends at the end of the deletion
        request page.
        :param item: a QID which should be deleted
        :type item: string
        :param reason: short text about the reason for the deletion request
        :type reason: str
        :param login: A WDI login object which contains username and password the edit should be performed with.
        :type login: wdi_login.WDLogin
        """

# SPARQL and API queries

##  execute_sparql_query

    @staticmethod
    @wdi_backoff()
    def execute_sparql_query(query, prefix=None, endpoint=None,
                             user_agent=None, as_dataframe=False, max_retries=1000, retry_after=60):
        """
        Static method which can be used to execute any SPARQL query
        :param prefix: The URI prefixes required for an endpoint, default is the Wikidata specific prefixes
        :param query: The actual SPARQL query string
        :param endpoint: The URL string for the SPARQL endpoint. Default is the URL for the Wikidata SPARQL endpoint
        :param user_agent: Set a user agent string for the HTTP header to let the WDQS know who you are.
        :param as_dataframe: Return result as pandas dataframe
        :type user_agent: str
        :param max_retries: The number time this function should retry in case of header reports.
        :param retry_after: the number of seconds should wait upon receiving either an error code or the WDQS is not reachable.
        :return: The results of the query are returned in JSON format
        """

##  `_sparql_query_result_to_df`

    @staticmethod
      def _sparql_query_result_to_df(results)

## get_linked_by

    @staticmethod
    def get_linked_by(qid, mediawiki_api_url=None):
        """
            :param qid: Wikidata identifier to which other wikidata items link
            :param mediawiki_api_url: default to wikidata's api, but can be changed to any wikibase
            :return:
        """
## mediawiki_api_call

    @staticmethod
    def mediawiki_api_call(method, mediawiki_api_url=None,
                           session=None, max_retries=1000, retry_after=60, **kwargs):
        """
        :param method: 'GET' or 'POST'
        :param mediawiki_api_url:
        :param session: If a session is passed, it will be used. Otherwise a new requests session is created
        :param max_retries: If api request fails due to rate limiting, maxlag, or readonly mode, retry up to
        `max_retries` times
        :type max_retries: int
        :param retry_after: Number of seconds to wait before retrying request (see max_retries)
        :type retry_after: int
        :param kwargs: Passed to requests.request
        :return:
        """

# Helpers 

## wikibase_item_engine_factory
    @classmethod
    def wikibase_item_engine_factory(cls, mediawiki_api_url=config['MEDIAWIKI_API_URL'],
                                     sparql_endpoint_url=config['SPARQL_ENDPOINT_URL'], name='LocalItemEngine'):
        """
        Helper function for creating a WDItemEngine class with arguments set for a different Wikibase instance than
        Wikidata.
        :param mediawiki_api_url: Mediawiki api url. For wikidata, this is: 'https://www.wikidata.org/w/api.php'
        :param sparql_endpoint_url: sparql endpoint url. For wikidata, this is: 'https://query.wikidata.org/sparql'
        :param name: name of the resulting class
        :return: a subclass of WDItemEngine with the mediawiki_api_url and sparql_endpoint_url arguments set
        """


## setup_logging
    @classmethod
    def setup_logging(cls, log_dir="./logs", log_name=None, header=None, names=None,
                      delimiter=";", logger_name='WD_logger'):
        """
        A static method which initiates log files compatible to .csv format, allowing for easy further analysis.
        :param log_dir: allows for setting relative or absolute path for logging, default is ./logs.
        :type log_dir: str
        :param log_name: File name of log file to be written. e.g. "WD_bot_run-20160204.log". Default is "WD_bot_run"
        and a timestamp of the current time
        :type log_name: str
        :param header: Log file will be prepended with header if given
        :type header: str
        :param names: Column names for the log file
        :type names: list
        :param delimiter: Log file will be delimited with `delimiter`
        :type delimiter: str
        """
 ## log
 
    @classmethod
    def log(cls, level, message):
        """
        :param level: The log level as in the Python logging documentation, 5 different possible values with increasing
         severity
        :type level: String of value 'DEBUG', 'INFO', 'WARNING', 'ERROR' or 'CRITICAL'.
        :param message: The logging data which should be written to the log file. In order to achieve a csv-file
         compatible format, all fields must be separated by a colon. Furthermore, all strings which could contain
         colons, spaces or other special characters must be enclosed in double-quotes.
         e.g. '{main_data_id}, "{exception_type}", "{message}", {wd_id}, {duration}'.format(
                        main_data_id=<main_id>,
                        exception_type=<excpetion type>,
                        message=<exception message>,
                        wd_id=<wikidata id>,
                        duration=<duration of action>
        :type message: str
        """
        
## generate_item_instances

    @classmethod
    def generate_item_instances(cls, items, mediawiki_api_url=None, login=None,
                                user_agent=None):
        """
        A method which allows for retrieval of a list of Wikidata items or properties. The method generates a list of
        tuples where the first value in the tuple is the QID or property ID, whereas the second is the new instance of
        WDItemEngine containing all the data of the item. This is most useful for mass retrieval of WD items.
        :param items: A list of QIDs or property IDs
        :type items: list
        :param mediawiki_api_url: The MediaWiki url which should be used
        :type mediawiki_api_url: str
        :param login: An object of type WDLogin, which holds the credentials/session cookies required for >50 item bulk
            retrieval of items.
        :type login: wdi_login.WDLogin
        :return: A list of tuples, first value in the tuple is the QID or property ID string, second value is the
            instance of WDItemEngine with the corresponding item data.
        """
      
## get_distinct_value_props

```
  @classmethod
  def get_distinct_value_props(cls, sparql_endpoint_url=None, wikibase_url=None, property_constraint_pid=None,
                                 distinct_values_constraint_qid=None):
        """
        On wikidata, the default core IDs will be the properties with a distinct values constraint
        select ?p where {?p wdt:P2302 wd:Q21502410}
        See: https://www.wikidata.org/wiki/Help:Property_constraints_portal
        https://www.wikidata.org/wiki/Help:Property_constraints_portal/Unique_value
        """
```

## init_data_load

Wrapper for a set of methods

## init_fastrun

## rollback
```
   def rollback(self, login, bot_account=True, summary="")
```
## get_wd_entity

```
    def get_wd_entity(self):
        """
        retrieve a WD item in json representation from Wikidata
        :rtype: dict
        :return: python complex dictionary represenation of a json
        """
```
## parse_wd_json

```
    def parse_wd_json(self, wd_json):
        """
        Parses a WD entity json and generates the datatype objects, sets self.wd_json_representation
        :param wd_json: the json of a WD entity
        :type wd_json: A Python Json representation of a WD item
        :return: returns the json representation containing 'labels', 'descriptions', 'claims', 'aliases', 'sitelinks'.
        """
```

## get_wd_search_results

```
    @staticmethod
    def get_wd_search_results(search_string='', mediawiki_api_url=None,
                              user_agent=None, max_results=500,
                              language='en', dict_id_label=False, dict_id_all_info=False):
           """
        Performs a search in WD for a certain WD search string
        :param search_string: a string which should be searched for in WD
        :type search_string: str
        :param mediawiki_api_url: Specify the mediawiki_api_url.
        :type mediawiki_api_url: str
        :param user_agent: The user agent string transmitted in the http header
        :type user_agent: str
        :param max_results: The maximum number of search results returned. Default 500
        :type max_results: int
        :param language: The language in which to perform the search. Default 'en'
        :type language: str
        :return: returns a list of QIDs found in the search and a list of labels complementary to the QIDs
        :type dict_id_label: boolean
        :return: returns a list of QIDs found in the search and a list of labels, descriptions, and wikidata urls complementary to the QIDs
        :type dict_id_all_info: boolean
        :return: function return a list with a dict of id, label, description, and url
        """
```

##  get_property_list

```
    def get_property_list(self):
        """
        List of properties on the current item
        :return: a list of WD property ID strings (Pxxxx).
        """
```

##  __select_wd_item__

```
    def __select_wd_item(self):
        """
        The most likely WD item QID should be returned, after querying WDQ for all values in core_id properties
        :return: Either a single WD QID is returned, or an empty string if no suitable item in WD
        """
```

## __construct_claim_json

```
    def __construct_claim_json(self):
        """
        Writes the properties from self.data to a new or existing json in self.wd_json_representation
        :return: None
        """
```


## __check_integrity
```
    def __check_integrity(self):
        """
        A method to check if when invoking __select_wd_item() and the WD item does not exist yet, but another item
        has a property of the current domain with a value like submitted in the data dict, this item does not get
        selected but a ManualInterventionReqException() is raised. This check is dependent on the core identifiers
        of a certain domain.
        :return: boolean True if test passed

```

# Setting non-data elements of Wikidata Item

## set_label

```
   def set_label(self, label, lang='en'):
        """
        Set the label for a WD item in a certain language
        :param label: The description of the item in a certain language
        :type label: str
        :param lang: The language a label should be set for.
        :type lang: str
        :return: None
        """
```

## set_aliases     

    def set_aliases(self, aliases, lang='en', append=True):
        """
        set the aliases for a WD item
        :param aliases: a list of strings representing the aliases of a WD item
        :param lang: The language a description should be set for
        :param append: If true, append a new alias to the list of existing aliases, else, overwrite. Default: True
        :return: None
        """

## set_descriptions

    def set_description(self, description, lang='en'):
        """
        Set the description for a WD item in a certain language
        :param description: The description of the item in a certain language
        :type description: str
        :param lang: The language a description should be set for.
        :type lang: str
        :return: None
        """

## set_sitelink
    def set_sitelink(self, site, title, badges=()):
        """
        Set sitelinks to corresponding Wikipedia pages
        :param site: The Wikipedia page a sitelink is directed to (e.g. 'enwiki')
        :param title: The title of the Wikipedia page the sitelink is directed to
        :param badges: An iterable containing Wikipedia badge strings.
        :return:
        """

# Accessor methods

## get_wd_json_representation

```
    def get_wd_json_representation(self):
        """
        A method to access the internal json representation of the WD item, mainly for testing
        :return: returns a Python json representation object of the WD item at the current state of the instance
        """
```
## get_pageid(self)
```
  def get_pageid(self):
        """
        Returns the pageid of a Wikidata item
        :return:
        """
```

## get_label

```
    def get_label(self, lang='en'):
        """
        Returns the label for a certain language
        :param lang:
        :type lang: str
        :return: returns the label in the specified language, an empty string if the label does not exist
        """
```
## get_aliases
```
    def get_aliases(self, lang='en'):
        """
        Retrieve the aliases in a certain language
        :param lang: The Wikidata language the description should be retrieved for
        :retur
```

## get_description

    def get_description(self, lang='en'):
        """
        Retrieve the description in a certain language
        :param lang: The Wikidata language the description should be retrieved for
        :return: 

## get_sitelink
 
 
    def get_sitelink(self, site):
        """
        A method to access the interwiki links in the json.model
        :param site: The Wikipedia site the interwiki/sitelink should be returned for
        :return: The interwiki/sitelink string for the specified Wikipedia will be returned.
        """
