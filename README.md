# SPARQL-cheatsheet 
All those SPARQL commands & tricks that you keep on forgetting ヽ༼ຈل͜ຈ༽ﾉ

**Contribuitons are welcomed. Send you pull requests** 


## FILTER


### FILTER EXISTS
Returns graph patterns that 
* Match the condition. `FILTER EXISTS {?subject rdfs:range rdfs:Literal}`
* Do not match the condition `FILTER NOT EXISTS {?subject rdfs:range rdfs:Literal}`
Example: return all the subjects with rdfs:range rdfs:Literal
```SPARQL
SELECT DISTINCT ?subject ?predicate ?range
WHERE {
    ?subject rdf:type ?predicate.
    ?subject rdfs:range ?range.
    FILTER EXISTS {?subject rdfs:range rdfs:Literal} 
}
``` 


### FILTER blank nodes: [isBlank](https://www.w3.org/TR/rdf-sparql-query/#func-isBlank)

* filter in only bank nodes `FILTER (isBlank(?term))` 
* filter out `FILTER (!isBlank(?term))` 
``` SPARQL
SELECT DISTINCT ?subject ?predicate
WHERE {
    ?subject rdf:type ?predicate.
    FILTER (isBlank(?subject)) 
}
```


### FILTER [isIRI](https://www.w3.org/TR/rdf-sparql-query/#func-isIRI)


### FILTER [isLiteral](https://www.w3.org/TR/rdf-sparql-query/#func-isLiteral)


### FILTER [lang](https://www.w3.org/TR/rdf-sparql-query/#func-lang)


### FILTER [datatype](https://www.w3.org/TR/rdf-sparql-query/#func-datatype)


----

# SPARQL Resources
* DuCharme, Bob. Learning SPARQL: Querying and Updating with SPARQL 1.1. Second edition. Sebastopol, CA: O’Reilly Media, 2013.
* “SPARQL 1.1 Query Language.” https://www.w3.org/TR/sparql11-query/.
* “Bob DuCharme Blog.” http://www.bobdc.com/blog/.
* “Module: SPARQL — Web Portal for RDF.Rb.” http://rdf.greggkellogg.net/yard/SPARQL.html.




