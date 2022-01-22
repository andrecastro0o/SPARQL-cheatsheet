# SPARQL-cheatsheet 
All those SPARQL commands & tricks that you keep on forgetting ヽ༼ຈل͜ຈ༽ﾉ

**Contribuitons are welcomed. Send you pull requests** 

## Query Forms

### SELECT
...

### CONSTRUCT
Returns triples - new values can be created through it

within the `CONSTRUCT` triples are created based on the variables established within the `WHERE` query  
```SPARQL
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX mo: <http://semantics.id/ns/example/movie#>
CONSTRUCT
    { 
     ?s a mo:Movie ;
        mo:hasWikidataLink ?s ;
        mo:hasIMDBResource ?imdbRes ;
        mo:title ?title . 
    }
WHERE {
    SERVICE <https://query.wikidata.org/sparql>
    {
        ?s wdt:P31 wd:Q24869 ; #instance_of feature film  
        wdt:P345 ?imdbId ; 
        wdt:P1476 ?title .
        BIND(IRI(CONCAT("https://www.imdb.com/title/", ?imdbId )) AS ?imdbRes) .
    }
}
LIMIT 2
```
Resulting in new triples
```ttl
@prefix mo:  <http://semantics.id/ns/example/movie#> .
@prefix wd:  <http://www.wikidata.org/entity/> .
@prefix wdt: <http://www.wikidata.org/prop/direct/> .

wd:Q965111  a               mo:Movie ;
        mo:hasIMDBResource  <https://www.imdb.com/title/tt1079448> ;
        mo:hasWikidataLink  wd:Q965111 ;
        mo:title            "Snow Buddies"@en .

wd:Q964009  a               mo:Movie ;
        mo:hasIMDBResource  <https://www.imdb.com/title/tt0070215> ;
        mo:hasWikidataLink  wd:Q964009 ;
        mo:title            "Il mio nome è Nessuno"@it .
```
### ASK

### DESCRIBE 


## String Operations
* `UCASE()` convert to upper-case 
* `LCASE()` convert to lower-case
* `STRLEN()` lenght of a string
* `SUBSTR()` substring of first arg, second arg: start position; third arg: character lenght; 
* `STRSTARTS()`string in first arg starts with string in second arg
* `STRENDS()`  string in first arg ends with string in second arg
* `CONTAINS()` string in second arg is within first arg. Example: `CONTAINS("SPARQL", "QL")` is True. `FILTER(CONTAINS(str(?term), "iao") || CONTAINS(str(?term), "bfo") || CONTAINS(str(?term), "obo")) ` 

## Logic operators
* `sameTerm` & `!sameTerm`  (more efficient and `=`)
* `=`, `!=`, `>`, `>=`, `<`, `<=`
* AND: `&&` OR: `||`
* `IN` and `NOT IN`: `?foo IN ("bar", "anotherbar")`
* [isIRI](https://www.w3.org/TR/rdf-sparql-query/#func-isIRI) `isIRI`
* [isBlank](https://www.w3.org/TR/rdf-sparql-query/#func-isBlank): only bank nodes `FILTER (isBlank(?term))`  filter out `FILTER (!isBlank(?term))`  
* [isLiteral](https://www.w3.org/TR/rdf-sparql-query/#func-isLiteral)


## Functions
* [lang](https://www.w3.org/TR/rdf-sparql-query/#func-lang) Returns the language tag. 
    * `FILTER(lang(?label) = 'es')`
    * *any* `@en` variant (will match all of these: `@en`, `@en-US`, `@en-GB`) `FILTER(langMatches(lang(?label), 'en'))`



## Aggregation Functions
* `COUNT`
* `SUM`
* `AVG`
* `MIN`, `MAX`
* `SAMPLE`
* `GROUP_CONCAT`

----

# Tips to speedup SPARQL Queries:
* reduce search space
* avoid `OPTIONAL` clauses (when possible), or have them follown non-optional clauses
* avoid `FILTER` or try having them earlier - so search space is reduced:
    * `FILTER(sameTerm(?term1, ?term2))`  is faster that `FILTER(?terms = ?term)`
    * idem for `!sameTerm` being faster that `!=`
    * regexs are expensive 
* avoid `DISTINCT`
* avoid `SORT BY` in  large amount of results
* reduce printout named variables - allows for more optimization flexibility from the SPARQL processor 

# SPARQL Resources
* “SPARQL Functions Reference — GraphDB Free 9.10.0 Documentation.”  https://graphdb.ontotext.com/documentation/free/sparql-functions-reference.html.
* DuCharme, Bob. Learning SPARQL: Querying and Updating with SPARQL 1.1. Second edition. Sebastopol, CA: O’Reilly Media, 2013.
* “SPARQL 1.1 Query Language.” https://www.w3.org/TR/sparql11-query/.
* “Bob DuCharme Blog.” http://www.bobdc.com/blog/.
* “Module: SPARQL — Web Portal for RDF.Rb.” http://rdf.greggkellogg.net/yard/SPARQL.html.
* “Wikidata:Request a Query - Wikidata.” https://www.wikidata.org/wiki/Wikidata:Request_a_query.





