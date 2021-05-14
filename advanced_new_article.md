# Advanced - Adding a new article to Wikidata using WDI

Now that you (hopefully) got to know the basics of Wikidata Integrator, let's dive into some cool functionalities.

Besides the `wdi_core` and the `wdi_login` submodules, for adding a new article to Wikidata we will need the submodule `wdi_helpers`.

The `wdi_helpers` module implements a class called `PublicationHelper` which helps you handle publications on Wikidata.

For the basic usage, you'll have to specify 3 parameters:

- `ext_id`: The external ID that will be used to get or create the item
- `id_type`: The type of external ID, might be one of {'pmid', 'pmcid', 'doi'}
- `source`: The external source of the metadata information that will be fulled to Wikidata. 

Here is one example of how you might go about creating a `PublicationHelper`:

```python3
from wikidataintegrator import wdi_core, wdi_login, wdi_helpers
login_instance = wdi_login.WDLogin(user=user, pwd=pwd)

wdi_publication_helper = wdi_helpers.PublicationHelper(
                                    ext_id="10.1111/aen.12530",
                                    id_type="doi",
                                    source="crossref")
```

Now we will run a method of PublicationHelper, `PublicationHelper.get_or_create()`. 
As the name says, if the ID is already on Wikidata, it will return the item related to that ID.
It does so in a tuple:

```python3
wdi_publication_helper.get_or_create(login_instance)
# ('Q106255296', [], True)
```

Let's try now with a DOI that is not on Wikidata yet: "10.7554/eLife.67077"



```python3
from wikidataintegrator import wdi_core, wdi_login, wdi_helpers
login_instance = wdi_login.WDLogin(user=user, pwd=pwd)

wdi_publication_helper = wdi_helpers.PublicationHelper(
                                    ext_id="10.7554/eLife.67077",
                                    id_type="doi",
                                    source="crossref")
                                    
wdi_publication_helper.get_or_create(login_instance)

# ('Q106832634', [], True)
```
The output looks similar, as the method returns the ID of the newly created item. However, something else happened: 
the item about the article  was created, with a lot of (good) information pulled from crossref. 
You can see that edit [here](https://www.wikidata.org/w/index.php?title=Q106832634&oldid=1420451380).



