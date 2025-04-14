# Parmigiano
This folder contains:
- The Lemma Bank of the [dialect of Parma]([https://en.wikipedia.org/wiki/Parma](https://it.wikipedia.org/wiki/Dialetto_parmigiano)). It consists of a large collection of lemmas, serving as the backbone to achieve interoperability, by linking all those entries in lexical resources and tokens in corpora that point to the same lemma.
- The Carpi-Pavarini glossary, that is a bilingual lexicon having Italian entries and the corresponding translations in Parmigiano.
Data are modelled according to the OntoLex-Lemon model and are provided in Turtle format. The RDF version of the glossary includes the linking to the LiIta Knowledge Base.
The subfolder *source* contains the same data but in csv format.

## Endpoint
Data can be queried through the following endpoint: [https://kgccc.di.unito.it/sparql/](https://kgccc.di.unito.it/sparql/).

## SPARQL queries
This section provides a set of SPARQL queries to be used on the aforementioned endpoint.

**Find all entries in Parmigiano marked as jargon.**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?lemma ?wr
WHERE {
?lemma ontolex:writtenRep ?wr ;
dcterms:isPartOf <http://liita.it/data/id/DialettoParmigiano/lemma/LemmaBank> .
?entry a ontolex:LexicalEntry ;
rdfs:comment "Usage: Voce gergale" ;
ontolex:canonicalForm ?lemma .
}
```
**Find all Italian entries that contain the substring *vecch* (root meaning *old*) and show the corresponding translations into Parmigiano**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT)
WHERE {
?lemma a lila:Lemma .
?lemma ontolex:writtenRep ?wr .
?le ontolex:canonicalForm ?lemma.
?leITA ontolex:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
FILTER regex(str(?wrIT), "vecch") .
} group by ?lemma ?liitaLemma
```
**Find entries that starts with *z* in Parmigiano and with *s* in Italian**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT)
WHERE {
?lemma a lila:Lemma .
?lemma ontolex:writtenRep ?wr .
?le ontolex:canonicalForm ?lemma.
?leITA ontolex:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
FILTER regex(str(?wr), "^z") .
FILTER regex(str(?wrIT), "^s") .
} group by ?lemma ?liitaLemma
```
**Find entries that contains *ci* (voiceless postalveolar affricate) in Parmigiano and *chi* (voiceless velar plosive) in Italian**
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT)
WHERE {
?lemma a lila:Lemma .
?lemma ontolex:writtenRep ?wr .
?le ontolex:canonicalForm ?lemma.
?leITA ontolex:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
FILTER regex(str(?wr), "ci") .
FILTER regex(str(?wrIT), "chi") .
} group by ?lemma ?liitaLemma
```
**Find adjectives that end with *bil* in Parmigiano** 
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT)
WHERE {
?lemma a lila:Lemma .
?lemma ontolex:writtenRep ?wr .
?lemma lila:hasPOS lila:adjective .
?le ontolex:canonicalForm ?lemma.
?leITA ontolex:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
FILTER regex(str(?wr), "bil$") .
} group by ?lemma ?liitaLemma
```
**Query the [Compl-IT lexicon](https://dspace-clarin-it.ilc.cnr.it/repository/xmlui/handle/20.500.11752/ILC-1007) to find definitions that begin with the word *uccello* (EN: *bird*), the correspoding Italian entry and the translation into Parmigiano**
```
PREFIX lime: <http://www.w3.org/ns/lemon/lime#>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX dct: <http://purl.org/dc/terms/>
PREFIX onto: <http://www.ontotext.com/>
PREFIX lexinfo: <http://www.lexinfo.net/ontology/3.0/lexinfo#>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
SELECT ?liitaLemma ?lemma ?parmigianoLemma ?wr ?definition WHERE
{
  SERVICE <https://klab.ilc.cnr.it/graphdb-compl-it/> {
    ?word a ontolex:Word ;
  lexinfo:partOfSpeech [ rdfs:label ?pos ] ;
                         ontolex:sense ?sense ;
                         ontolex:canonicalForm ?form .
    ?form ontolex:writtenRep ?lemma .
    OPTIONAL {
      ?sense skos:definition ?definition 
    } .
    OPTIONAL {
      ?sense lexinfo:senseExample ?example 
    } .
    FILTER(str(?pos) = "noun") .
    FILTER (strstarts(str(?definition), "uccello")).
  }
  ?word ontolex:canonicalForm ?liitaLemma.
  ?leItaLexiconPar ontolex:canonicalForm ?liitaLemma;
                   ^lime:entry <http://liita.it/data/LexicalReources/DialettoParmigiano/Lexicon>.
  ?leItaLexiconPar ontolex:translatableAs ?leParLexiconPar.
  ?leParLexiconPar ontolex:canonicalForm ?parmigianoLemma.
  ?parmigianoLemma ontolex:writtenRep ?wr
}  group by ?wr
```
