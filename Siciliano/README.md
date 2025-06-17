# Siciliano

This folder contains:

- The Lemma Bank of Sicilian. It consists of a large collection of lemmas, serving as the backbone to achieve interoperability, by linking all those entries in lexical resources and tokens in corpora that point to the same lemma.
- A Sicilian-Italian lexicon extracted from [wikizziunariu](https://scn.wiktionary.org/wiki/P%C3%A0ggina_principali). Data are modelled according to the OntoLex-Lemon model and are provided in Turtle format. The RDF version of the glossary includes the linking to the LiIta Knowledge Base. The subfolder source contains the same data but in tsv format.

## Endpoint
Data can be queried through the following endpoint: [https://kgccc.di.unito.it/sparql/](https://kgccc.di.unito.it/sparql/).

## SPARQL queries
This section provides a set of SPARQL queries to be used on the aforementioned endpoint.

**Find all Sicilian lemmas having written representations containing the alternation "ed"-"ied"**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>

SELECT ?lemmaSiciliano ?lemmaSicilianoLabel ?wr1 ?wr2
WHERE {
?lemmaSiciliano dcterms:isPartOf <http://liita.it/data/id/DialettoSiciliano/lemma/LemmaBank>.
?lemmaSiciliano rdfs:label ?lemmaSicilianoLabel.
?lemmaSiciliano ontolex:writtenRep ?wr1, ?wr2 .
filter( ?wr1 != ?wr2 ).
FILTER regex(str(?wr1), "ied") .
FILTER regex(str(?wr2), "ed") .
} group by ?lemmaSiciliano
```

**Find all Sicilian lemmas having written representations starting with "d" or "r"**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>

SELECT ?lemmaSiciliano ?lemmaSicilianoLabel
WHERE {
?lemmaSiciliano dcterms:isPartOf <http://liita.it/data/id/DialettoSiciliano/lemma/LemmaBank>.
?lemmaSiciliano rdfs:label ?lemmaSicilianoLabel.
?lemmaSiciliano ontolex:writtenRep ?wr1, ?wr2 .
filter( ?wr1 != ?wr2 ).
FILTER regex(str(?wr1), "^d") .
FILTER regex(str(?wr2), "^r") .
} group by ?lemmaSiciliano
```

**Find Sicilian common nouns ending with the abstract suffix "ìa" and show the correspoding Italian translations**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT)
WHERE {
?lemma dcterms:isPartOf <http://liita.it/data/id/DialettoSiciliano/lemma/LemmaBank>.
?lemma ontolex:writtenRep ?wr .
?lemma lila:hasPOS lila:noun .
?le ontolex:canonicalForm ?lemma.
?leITA vartrans:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
FILTER regex(str(?wr), "ìa$") .
} group by ?lemma ?liitaLemma
```
