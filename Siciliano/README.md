# Siciliano

This folder contains:

- The Lemma Bank of Sicilian. It consists of a large collection of lemmas, serving as the backbone to achieve interoperability, by linking all those entries in lexical resources and tokens in corpora that point to the same lemma.
- A Sicilian-Italian lexicon extracted from [wikizziunariu](https://scn.wiktionary.org/wiki/P%C3%A0ggina_principali). Data are modelled according to the OntoLex-Lemon model and are provided in Turtle format. The RDF version of the glossary includes the linking to the LiIta Knowledge Base. The subfolder source contains the same data but in tsv format.
- The Sicilian Treebank (STB), a small parallel corpus of Sicilian texts, automatically parsed and then manually revised, with Italian translations. It includes both contemporary and folkloric materials. The treebank is taken from [Universal Dependencies](https://universaldependencies.org); the original files are available in a dedicated [repository](https://github.com/UniversalDependencies/UD_Sicilian-STB/tree/master).

## Endpoint
Data can be queried through the following endpoint: [https://liita.it/sparql](https://liita.it/sparql).

## SPARQL queries
This section provides a set of SPARQL queries to be used on the aforementioned endpoint.

**Find all Sicilian lemmas having written representations containing the alternation "ed"-"ied"**

[Results](https://liita.it/sparql?default-graph-uri=&query=PREFIX+lila%3A+%3Chttp%3A%2F%2Flila-erc.eu%2Fontologies%2Flila%2F%3E%0D%0APREFIX+ontolex%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fontolex%23%3E%0D%0A%0D%0ASELECT+%3FlemmaSiciliano+%3FlemmaSicilianoLabel+%3Fwr1+%3Fwr2%0D%0AWHERE+%7B%0D%0A%3FlemmaSiciliano+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoSiciliano%2Flemma%2FLemmaBank%3E.%0D%0A%3FlemmaSiciliano+rdfs%3Alabel+%3FlemmaSicilianoLabel.%0D%0A%3FlemmaSiciliano+ontolex%3AwrittenRep+%3Fwr1%2C+%3Fwr2+.%0D%0Afilter%28+%3Fwr1+%21%3D+%3Fwr2+%29.%0D%0AFILTER+regex%28str%28%3Fwr1%29%2C+%22ied%22%29+.%0D%0AFILTER+regex%28str%28%3Fwr2%29%2C+%22ed%22%29+.%0D%0A%7D+group+by+%3FlemmaSiciliano&format=text%2Fhtml&should-sponge=&timeout=0&signal_void=on)
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

[Results](https://liita.it/sparql?default-graph-uri=&query=PREFIX+lila%3A+%3Chttp%3A%2F%2Flila-erc.eu%2Fontologies%2Flila%2F%3E%0D%0APREFIX+ontolex%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fontolex%23%3E%0D%0A%0D%0ASELECT+%3FlemmaSiciliano+%3FlemmaSicilianoLabel%0D%0AWHERE+%7B%0D%0A%3FlemmaSiciliano+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoSiciliano%2Flemma%2FLemmaBank%3E.%0D%0A%3FlemmaSiciliano+rdfs%3Alabel+%3FlemmaSicilianoLabel.%0D%0A%3FlemmaSiciliano+ontolex%3AwrittenRep+%3Fwr1%2C+%3Fwr2+.%0D%0Afilter%28+%3Fwr1+%21%3D+%3Fwr2+%29.%0D%0AFILTER+regex%28str%28%3Fwr1%29%2C+%22%5Ed%22%29+.%0D%0AFILTER+regex%28str%28%3Fwr2%29%2C+%22%5Er%22%29+.%0D%0A%7D+group+by+%3FlemmaSiciliano&format=text%2Fhtml&should-sponge=&timeout=0&signal_void=on)
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

[Results](https://liita.it/sparql?default-graph-uri=&query=PREFIX+lila%3A+%3Chttp%3A%2F%2Flila-erc.eu%2Fontologies%2Flila%2F%3E%0D%0APREFIX+ontolex%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fontolex%23%3E%0D%0APREFIX+dcterms%3A+%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Fterms%2F%3E%0D%0APREFIX+vartrans%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fvartrans%23%3E%0D%0A%0D%0ASELECT+%3Flemma+%28GROUP_CONCAT%28DISTINCT+%3Fwr+%3Bseparator%3D%22%2C+%22%29+as+%3Fwrs%29+%3FliitaLemma+%28GROUP_CONCAT%28DISTINCT+%3FwrIT+%3Bseparator%3D%22%2C+%22%29+as+%3FwrsIT%29%0D%0AWHERE+%7B%0D%0A%3Flemma+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoSiciliano%2Flemma%2FLemmaBank%3E.%0D%0A%3Flemma+ontolex%3AwrittenRep+%3Fwr+.%0D%0A%3Flemma+lila%3AhasPOS+lila%3Anoun+.%0D%0A%3Fle+ontolex%3AcanonicalForm+%3Flemma.%0D%0A%3FleITA+vartrans%3AtranslatableAs+%3Fle%3B%0D%0Aontolex%3AcanonicalForm+%3FliitaLemma.%0D%0A%3FliitaLemma+ontolex%3AwrittenRep+%3FwrIT.%0D%0AFILTER+regex%28str%28%3Fwr%29%2C+%22%C3%ACa%24%22%29+.%0D%0A%7D+group+by+%3Flemma+%3FliitaLemma&format=text%2Fhtml&should-sponge=&timeout=0&signal_void=on)
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

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

**Find Sicilian entries having a feminine gender which Italian translation is masculine**

[Results](https://liita.it/sparql?default-graph-uri=&query=PREFIX+lila%3A+%3Chttp%3A%2F%2Flila-erc.eu%2Fontologies%2Flila%2F%3E%0D%0APREFIX+ontolex%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fontolex%23%3E%0D%0APREFIX+dcterms%3A+%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Fterms%2F%3E%0D%0APREFIX+vartrans%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fvartrans%23%3E%0D%0A%0D%0ASELECT+%3Flemma+%28GROUP_CONCAT%28DISTINCT+%3Fwr+%3Bseparator%3D%22%2C+%22%29+as+%3Fwrs%29+%3FliitaLemma+%28GROUP_CONCAT%28DISTINCT+%3FwrIT+%3Bseparator%3D%22%2C+%22%29+as+%3FwrsIT%29%0D%0AWHERE+%7B%0D%0A%3Flemma+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoSiciliano%2Flemma%2FLemmaBank%3E.%0D%0A%3Flemma+ontolex%3AwrittenRep+%3Fwr+.%0D%0A%3Flemma+lila%3AhasGender+lila%3Afeminine.%0D%0A%3Fle+ontolex%3AcanonicalForm+%3Flemma.%0D%0A%3FleITA+vartrans%3AtranslatableAs+%3Fle%3B%0D%0Aontolex%3AcanonicalForm+%3FliitaLemma.%0D%0A%3FliitaLemma+ontolex%3AwrittenRep+%3FwrIT.%0D%0A%3FliitaLemma+lila%3AhasGender+lila%3Amasculine.%0D%0A%7D+group+by+%3Flemma+%3FliitaLemma&format=text%2Fhtml&should-sponge=&timeout=0&signal_void=on)
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT)
WHERE {
?lemma dcterms:isPartOf <http://liita.it/data/id/DialettoSiciliano/lemma/LemmaBank>.
?lemma ontolex:writtenRep ?wr .
?lemma lila:hasGender lila:feminine.
?le ontolex:canonicalForm ?lemma.
?leITA vartrans:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
?liitaLemma lila:hasGender lila:masculine.
} group by ?lemma ?liitaLemma
```

**Find Italian verbs of the first conjugation (ending with "are") and shows the corresponding translations in Parmigiano and Sicilian**

[Results](https://liita.it/sparql?default-graph-uri=&query=PREFIX+lila%3A+%3Chttp%3A%2F%2Flila-erc.eu%2Fontologies%2Flila%2F%3E%0D%0APREFIX+ontolex%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fontolex%23%3E%0D%0APREFIX+dcterms%3A+%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Fterms%2F%3E%0D%0APREFIX+vartrans%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fvartrans%23%3E%0D%0A%0D%0ASELECT+%3Flemma+%28GROUP_CONCAT%28DISTINCT+%3Fwr+%3Bseparator%3D%22%2C+%22%29+as+%3Fwrs%29+%3FliitaLemma+%28GROUP_CONCAT%28DISTINCT+%3FwrIT+%3Bseparator%3D%22%2C+%22%29+as+%3FwrsIT%29+%3FlemmaPR+%28GROUP_CONCAT%28DISTINCT+%3FwrPR+%3Bseparator%3D%22%2C+%22%29+as+%3FwrsPR%29+%0D%0AWHERE+%7B%0D%0A%3Flemma+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoSiciliano%2Flemma%2FLemmaBank%3E.%0D%0A%3Flemma+ontolex%3AwrittenRep+%3Fwr+.%0D%0A%3Fle+ontolex%3AcanonicalForm+%3Flemma.%0D%0A%3FleITA+vartrans%3AtranslatableAs+%3Fle%3B%0D%0Aontolex%3AcanonicalForm+%3FliitaLemma.%0D%0A%3FliitaLemma+ontolex%3AwrittenRep+%3FwrIT.%0D%0A%3FliitaLemma+lila%3AhasPOS+lila%3Averb+.%0D%0A%3FleITAPR+ontolex%3AcanonicalForm+%3FliitaLemma.%0D%0A%3FleITAPR+vartrans%3AtranslatableAs+%3FlePR+.%0D%0A%3FlePR+ontolex%3AcanonicalForm+%3FlemmaPR+.%0D%0A%3FlemmaPR+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoParmigiano%2Flemma%2FLemmaBank%3E.%0D%0A%3FlemmaPR+ontolex%3AwrittenRep+%3FwrPR+.%0D%0AFILTER+regex%28str%28%3FwrIT%29%2C+%22are%24%22%29+.%0D%0A%7D+group+by+%3FliitaLemma+%3Flemma+%3FlemmaPR&format=text%2Fhtml&should-sponge=&timeout=0&signal_void=on)
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;separator=", ") as ?wrsIT) ?lemmaPR (GROUP_CONCAT(DISTINCT ?wrPR ;separator=", ") as ?wrsPR) 
WHERE {
?lemma dcterms:isPartOf <http://liita.it/data/id/DialettoSiciliano/lemma/LemmaBank>.
?lemma ontolex:writtenRep ?wr .
?le ontolex:canonicalForm ?lemma.
?leITA vartrans:translatableAs ?le;
ontolex:canonicalForm ?liitaLemma.
?liitaLemma ontolex:writtenRep ?wrIT.
?liitaLemma lila:hasPOS lila:verb .
?leITAPR ontolex:canonicalForm ?liitaLemma.
?leITAPR vartrans:translatableAs ?lePR .
?lePR ontolex:canonicalForm ?lemmaPR .
?lemmaPR dcterms:isPartOf <http://liita.it/data/id/DialettoParmigiano/lemma/LemmaBank>.
?lemmaPR ontolex:writtenRep ?wrPR .
FILTER regex(str(?wrIT), "are$") .
} group by ?liitaLemma ?lemma ?lemmaPR
```


**Find Italian adjectives ending with "-oso" and having a translation in Sicilian and in the Parma dialect showing the definition taken from the CompL-it lexicon**

[Results](https://liita.it/sparql?default-graph-uri=&query=PREFIX+lila%3A+%3Chttp%3A%2F%2Flila-erc.eu%2Fontologies%2Flila%2F%3E%0D%0APREFIX+ontolex%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fontolex%23%3E%0D%0APREFIX+dcterms%3A+%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Fterms%2F%3E%0D%0APREFIX+vartrans%3A+%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flemon%2Fvartrans%23%3E%0D%0APREFIX+lexinfo%3A+%3Chttp%3A%2F%2Fwww.lexinfo.net%2Fontology%2F3.0%2Flexinfo%23%3E%0D%0ASelect+%3FwrsIT+%3FliitaLemma++%3Fwrs++%3FwrsPR+%3FlemmaPR++%28GROUP_CONCAT%28DISTINCT+%3Fdefinition+%3B%0D%0A++++separator%3D%22%2C+%22%29+as+%3Fdefinitions%29++where+%7B%0D%0A++%7B%0D%0A++++SELECT+%3Flemma+%28GROUP_CONCAT%28DISTINCT+%3Fwr+%3B%0D%0A++++++++separator%3D%22%2C+%22%29+as+%3Fwrs%29+%3FliitaLemma+%28GROUP_CONCAT%28DISTINCT+%3FwrIT+%3B%0D%0A++++++++separator%3D%22%2C+%22%29+as+%3FwrsIT%29+%3FlemmaPR+%28GROUP_CONCAT%28DISTINCT+%3FwrPR+%3B%0D%0A++++++++separator%3D%22%2C+%22%29+as+%3FwrsPR+%29+%3Flabel_it%0D%0A++++WHERE+%7B%0D%0A++++++%3Flemma+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoSiciliano%2Flemma%2FLemmaBank%3E.%0D%0A++++++%3Flemma+ontolex%3AwrittenRep+%3Fwr+.%0D%0A++++++%3Fle+ontolex%3AcanonicalForm+%3Flemma.%0D%0A++++++%3FleITA+vartrans%3AtranslatableAs+%3Fle%3B%0D%0A+++++++++++++ontolex%3AcanonicalForm+%3FliitaLemma.%0D%0A++++++%3FliitaLemma+ontolex%3AwrittenRep+%3FwrIT.%0D%0A++++++%3FliitaLemma+lila%3AhasPOS+lila%3Aadjective+.%0D%0A++++++%3FleITAPR+ontolex%3AcanonicalForm+%3FliitaLemma.%0D%0A++++++%3FleITAPR+vartrans%3AtranslatableAs+%3FlePR+.%0D%0A++++++%3FlePR+ontolex%3AcanonicalForm+%3FlemmaPR+.%0D%0A++++++%3FlemmaPR+dcterms%3AisPartOf+%3Chttp%3A%2F%2Fliita.it%2Fdata%2Fid%2FDialettoParmigiano%2Flemma%2FLemmaBank%3E.%0D%0A++++++%3FlemmaPR+ontolex%3AwrittenRep+%3FwrPR+.%0D%0A++++++FILTER+regex%28str%28%3FwrIT%29%2C+%22oso%24%22%29+.%0D%0A++++++BIND%28+STRLANG%28str%28%3FwrIT%29%2C+%22it%22%29+AS+%3Flabel_it+%29+.%0D%0A++++%7D+group+by+%3FliitaLemma+%3Flemma+%3FlemmaPR+%3Flabel_it%0D%0A++%7D%0D%0A++SERVICE+%3Chttps%3A%2F%2Fklab.ilc.cnr.it%2Fgraphdb-compl-it%2F%3E+%7B%0D%0A++++%3Fword+a+ontolex%3AWord+%3B%0D%0A+++++++++++++++rdfs%3Alabel+%3Flabel_it%3B%0D%0A++++++++ontolex%3Asense+%3Fsense+%3B%0D%0A++++++++ontolex%3AcanonicalForm+%3Fform+.%0D%0A++++++++OPTIONAL+%7B%0D%0A++++++++%3Fsense+skos%3Adefinition+%3Fdefinition+%0D%0A++%7D+.%0D%0A++%7D%0D%0A++++++++%0D%0A++%7Dgroup+by+%3Fwrs+%3FliitaLemma+%3FwrsIT+%3FlemmaPR+%3FwrsPR+%0D%0Aorder+by++%3FwrsIT&format=text%2Fhtml&should-sponge=&timeout=0&signal_void=on)
```
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>
PREFIX lexinfo: <http://www.lexinfo.net/ontology/3.0/lexinfo#>
Select ?wrsIT ?liitaLemma  ?wrs  ?wrsPR ?lemmaPR  (GROUP_CONCAT(DISTINCT ?definition ;
    separator=", ") as ?definitions)  where {
  {
    SELECT ?lemma (GROUP_CONCAT(DISTINCT ?wr ;
        separator=", ") as ?wrs) ?liitaLemma (GROUP_CONCAT(DISTINCT ?wrIT ;
        separator=", ") as ?wrsIT) ?lemmaPR (GROUP_CONCAT(DISTINCT ?wrPR ;
        separator=", ") as ?wrsPR ) ?label_it
    WHERE {
      ?lemma dcterms:isPartOf <http://liita.it/data/id/DialettoSiciliano/lemma/LemmaBank>.
      ?lemma ontolex:writtenRep ?wr .
      ?le ontolex:canonicalForm ?lemma.
      ?leITA vartrans:translatableAs ?le;
             ontolex:canonicalForm ?liitaLemma.
      ?liitaLemma ontolex:writtenRep ?wrIT.
      ?liitaLemma lila:hasPOS lila:adjective .
      ?leITAPR ontolex:canonicalForm ?liitaLemma.
      ?leITAPR vartrans:translatableAs ?lePR .
      ?lePR ontolex:canonicalForm ?lemmaPR .
      ?lemmaPR dcterms:isPartOf <http://liita.it/data/id/DialettoParmigiano/lemma/LemmaBank>.
      ?lemmaPR ontolex:writtenRep ?wrPR .
      FILTER regex(str(?wrIT), "oso$") .
      BIND( STRLANG(str(?wrIT), "it") AS ?label_it ) .
    } group by ?liitaLemma ?lemma ?lemmaPR ?label_it
  }
  SERVICE <https://klab.ilc.cnr.it/graphdb-compl-it/> {
    ?word a ontolex:Word ;
               rdfs:label ?label_it;
        ontolex:sense ?sense ;
        ontolex:canonicalForm ?form .
        OPTIONAL {
        ?sense skos:definition ?definition 
  } .
  }
        
  }group by ?wrs ?liitaLemma ?wrsIT ?lemmaPR ?wrsPR 
order by  ?wrsIT
```

**Find the noun lemmas occurring as nominal subjects (nsubj) of the verb _diciri_ (to say) in the Sicilian Treebank**
```
PREFIX powla: <http://purl.org/powla/powla.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
prefix lilacorpora: <http://lila-erc.eu/ontologies/lila_corpora/>
prefix lila: <http://lila-erc.eu/ontologies/lila/>
prefix UDSynFunction: <https://universaldependencies.org/u/dep/>

SELECT ?subLemmaLabel ?docTitle
WHERE {
  VALUES ?copora {
    <http://liita.it/data/id/corpora/STB/id/corpus> 
  }
  VALUES ?synFunctions {
    UDSynFunction:nsubj
  }
  ?token rdf:type powla:Terminal;
         lila:hasLemma <http://liita.it/data/id/DialettoSiciliano/lemma/8101> .
  ?rel rdf:type ?synFunctions;
       lilacorpora:hasHead ?token ;
       lilacorpora:hasDep ?subj .
  ?subj lila:hasLemma ?subjLemma .
  ?token powla:hasLayer/powla:hasDocument/^powla:hasSubDocument ?copora .
  ?token powla:hasLayer/powla:hasDocument ?doc.
  ?doc dc:title ?docTitle .
  VALUES ?nounPos {
    lila:noun 
  }
  ?subjLemma lila:hasPOS ?nounPos .
  ?subjLemma rdfs:label ?subLemmaLabel.
}group by ?subjLemma ?subLemmaLabel ?docTitle
```

**Extract all adjective lemmas attested in the Sicilian Treebank, identify their Italian equivalents and, whenever available, the corresponding Parmigiano dialect translations**
```
PREFIX powla: <http://purl.org/powla/powla.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX lilacorpora: <http://lila-erc.eu/ontologies/lila_corpora/>
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

SELECT
  ?lemma
  (GROUP_CONCAT(DISTINCT ?wr; separator=", ") AS ?wrs)
  (GROUP_CONCAT(DISTINCT ?wrIT; separator=", ") AS ?wrsIT)
  (GROUP_CONCAT(DISTINCT ?wrPR; separator=", ") AS ?wrsPR)
WHERE {
  VALUES ?copora {
    <http://liita.it/data/id/corpora/STB/id/corpus>
  }

  ?token rdf:type powla:Terminal ;
         lila:hasLemma ?lemma .
  ?token powla:hasLayer/powla:hasDocument/^powla:hasSubDocument ?copora .

  ?lemma lila:hasPOS lila:adjective .
  ?lemma ontolex:writtenRep ?wr .

  OPTIONAL {
    ?le ontolex:canonicalForm ?lemma .
    ?leITA vartrans:translatableAs ?le ;
           ontolex:canonicalForm ?liitaLemmaIT .
    ?liitaLemmaIT ontolex:writtenRep ?wrIT .
    ?liitaLemmaIT dcterms:isPartOf <http://liita.it/data/id/lemma/LemmaBank> .

    OPTIONAL {
      ?leITA_parm ontolex:canonicalForm ?liitaLemmaIT .
      ?leITA_parm vartrans:translatableAs ?leParm .
      ?leParm ontolex:canonicalForm ?liitaLemmaPR .
      ?liitaLemmaPR ontolex:writtenRep ?wrPR .
      ?liitaLemmaPR dcterms:isPartOf <http://liita.it/data/id/DialettoParmigiano/lemma/LemmaBank> .
    }
  }
}
GROUP BY ?lemma
ORDER BY ?lemma
```

**Find Sicilian verbs attested in the Sicilian Treebank whose Italian translation ends in _-are_, and retrieve their corresponding Parmigiano equivalents**
```
PREFIX powla: <http://purl.org/powla/powla.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX lilacorpora: <http://lila-erc.eu/ontologies/lila_corpora/>
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

SELECT
  ?lemma
  (GROUP_CONCAT(DISTINCT ?wr; separator=", ") AS ?wrs)
  (GROUP_CONCAT(DISTINCT ?wrIT; separator=", ") AS ?wrsIT)
  (GROUP_CONCAT(DISTINCT ?wrPR; separator=", ") AS ?wrsPR)
WHERE {
  VALUES ?copora {
    <http://liita.it/data/id/corpora/STB/id/corpus>
  }

  ?token rdf:type powla:Terminal ;
         lila:hasLemma ?lemma .
  ?token powla:hasLayer/powla:hasDocument/^powla:hasSubDocument ?copora .

  ?lemma lila:hasPOS lila:verb .
  ?lemma ontolex:writtenRep ?wr .

  ?le ontolex:canonicalForm ?lemma .
  ?leITA vartrans:translatableAs ?le ;
         ontolex:canonicalForm ?liitaLemmaIT .
  ?liitaLemmaIT ontolex:writtenRep ?wrIT .
  ?liitaLemmaIT dcterms:isPartOf <http://liita.it/data/id/lemma/LemmaBank> .

  FILTER regex(str(?wrIT), "are$")

  OPTIONAL {
    ?leITA_parm ontolex:canonicalForm ?liitaLemmaIT .
    ?leITA_parm vartrans:translatableAs ?leParm .
    ?leParm ontolex:canonicalForm ?liitaLemmaPR .
    ?liitaLemmaPR ontolex:writtenRep ?wrPR .
    ?liitaLemmaPR dcterms:isPartOf <http://liita.it/data/id/DialettoParmigiano/lemma/LemmaBank> .
  }
}
GROUP BY ?lemma
ORDER BY ?lemma
```

**Find all verbs annotated with the imperative mood (Mood=Imp) in the STB corpus, returning the token form, its associated lemma, and, when available, the corresponding Italian translation**
```
PREFIX powla: <http://purl.org/powla/powla.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX oa: <http://www.w3.org/ns/oa#>
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

SELECT
  ?tokenLabel
  ?lemma
  ?wr
  (GROUP_CONCAT(DISTINCT ?wrIT; separator=", ") AS ?wrsIT)
WHERE {
  VALUES ?copora {
    <http://liita.it/data/id/corpora/STB/id/corpus>
  }

  ?annotation rdf:type oa:Annotation ;
              oa:hasBody <https://universaldependencies.org/it/feat/Mood#Imp> ;
              oa:hasTarget ?token .

  ?token rdf:type powla:Terminal ;
         lila:hasLemma ?lemma ;
         rdfs:label ?tokenLabel .

  ?token powla:hasLayer/powla:hasDocument/^powla:hasSubDocument ?copora .

  ?lemma lila:hasPOS lila:verb ;
         ontolex:writtenRep ?wr .

  OPTIONAL {
    ?le ontolex:canonicalForm ?lemma .
    ?leITA vartrans:translatableAs ?le ;
           ontolex:canonicalForm ?liitaLemmaIT .
    ?liitaLemmaIT ontolex:writtenRep ?wrIT .
    ?liitaLemmaIT dcterms:isPartOf <http://liita.it/data/id/lemma/LemmaBank> .
  }
}
GROUP BY ?tokenLabel ?lemma ?wr
ORDER BY ?lemma
```

**Find nouns, verbs, and adjectives that are exclusive to one of two Sicilian versions of the Colapisci text (Ganzirri 1904 vs. Roccalumera 1904) in the STB corpus**
```
PREFIX powla: <http://purl.org/powla/powla.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX vartrans: <http://www.w3.org/ns/lemon/vartrans#>

SELECT
  ?lemma
  ?lemmaLabel
  (GROUP_CONCAT(DISTINCT ?wrIT; separator=", ") AS ?wrsIT)
  ?soloIn
WHERE {
  VALUES ?pos { lila:noun lila:verb lila:adjective }

  {
    ?token rdf:type powla:Terminal ;
           lila:hasLemma ?lemma .
    ?token powla:hasLayer/powla:hasDocument <http://liita.it/data/id/corpora/STB/id/corpus/Colapisci%20-%20Ganzirri%201904> .

    FILTER NOT EXISTS {
      ?token2 rdf:type powla:Terminal ;
              lila:hasLemma ?lemma .
      ?token2 powla:hasLayer/powla:hasDocument <http://liita.it/data/id/corpora/STB/id/corpus/Colapisci%20-%20Roccalumera%201904> .
    }
    BIND("Ganzirri only" AS ?soloIn)
  }
  UNION
  {
    ?token rdf:type powla:Terminal ;
           lila:hasLemma ?lemma .
    ?token powla:hasLayer/powla:hasDocument <http://liita.it/data/id/corpora/STB/id/corpus/Colapisci%20-%20Roccalumera%201904> .

    FILTER NOT EXISTS {
      ?token2 rdf:type powla:Terminal ;
              lila:hasLemma ?lemma .
      ?token2 powla:hasLayer/powla:hasDocument <http://liita.it/data/id/corpora/STB/id/corpus/Colapisci%20-%20Ganzirri%201904> .
    }
    BIND("Roccalumera only" AS ?soloIn)
  }

  ?lemma rdfs:label ?lemmaLabel ;
         lila:hasPOS ?pos .

  OPTIONAL {
    ?le ontolex:canonicalForm ?lemma .
    ?leITA vartrans:translatableAs ?le ;
           ontolex:canonicalForm ?liitaLemmaIT .
    ?liitaLemmaIT ontolex:writtenRep ?wrIT .
    ?liitaLemmaIT dcterms:isPartOf <http://liita.it/data/id/lemma/LemmaBank> .
  }
}
GROUP BY ?lemma ?lemmaLabel ?soloIn
ORDER BY ?soloIn ?lemmaLabel
```

**Find all occurrences of the Sicilian lemma essiri (_to be_) in the Sicilian Treebank, together with their document of origin and the associated Universal Dependencies morphological features (e.g., mood, number, person, tense, and verb form). Duplicates are removed.**
```
PREFIX powla: <http://purl.org/powla/powla.owl#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX ontolex: <http://www.w3.org/ns/lemon/ontolex#>
PREFIX lila: <http://lila-erc.eu/ontologies/lila/>
PREFIX oa: <http://www.w3.org/ns/oa#>

SELECT DISTINCT
  (CONCAT(?mood, " | ", ?number, " | ", ?person, " | ", ?tense, " | ", ?verbform) AS ?features)
  ?tokenLabel ?docTitle
WHERE {
  VALUES ?copora {
    <http://liita.it/data/id/corpora/STB/id/corpus>
  }

  ?lemma a lila:Lemma ;
         ontolex:writtenRep "essiri"@scn .

  ?token rdf:type powla:Terminal ;
         lila:hasLemma ?lemma ;
         rdfs:label ?tokenLabel .

  ?token powla:hasLayer/powla:hasDocument/^powla:hasSubDocument ?copora .
  ?token powla:hasLayer/powla:hasDocument ?doc .
  ?doc dc:title ?docTitle .

  OPTIONAL {
    SELECT ?token (SAMPLE(?m) AS ?mood) WHERE {
      ?a oa:hasTarget ?token ; oa:hasBody ?b .
      FILTER(STRSTARTS(STR(?b), "https://universaldependencies.org/it/feat/Mood#"))
      BIND(STRAFTER(STR(?b), "/feat/") AS ?m)
    } GROUP BY ?token
  }
  OPTIONAL {
    SELECT ?token (SAMPLE(?n) AS ?number) WHERE {
      ?a oa:hasTarget ?token ; oa:hasBody ?b .
      FILTER(STRSTARTS(STR(?b), "https://universaldependencies.org/it/feat/Number#"))
      BIND(STRAFTER(STR(?b), "/feat/") AS ?n)
    } GROUP BY ?token
  }
  OPTIONAL {
    SELECT ?token (SAMPLE(?p) AS ?person) WHERE {
      ?a oa:hasTarget ?token ; oa:hasBody ?b .
      FILTER(STRSTARTS(STR(?b), "https://universaldependencies.org/it/feat/Person#"))
      BIND(STRAFTER(STR(?b), "/feat/") AS ?p)
    } GROUP BY ?token
  }
  OPTIONAL {
    SELECT ?token (SAMPLE(?t) AS ?tense) WHERE {
      ?a oa:hasTarget ?token ; oa:hasBody ?b .
      FILTER(STRSTARTS(STR(?b), "https://universaldependencies.org/it/feat/Tense#"))
      BIND(STRAFTER(STR(?b), "/feat/") AS ?t)
    } GROUP BY ?token
  }
  OPTIONAL {
    SELECT ?token (SAMPLE(?v) AS ?verbform) WHERE {
      ?a oa:hasTarget ?token ; oa:hasBody ?b .
      FILTER(STRSTARTS(STR(?b), "https://universaldependencies.org/it/feat/VerbForm#"))
      BIND(STRAFTER(STR(?b), "/feat/") AS ?v)
    } GROUP BY ?token
  }
}
ORDER BY DESC(?features) ?docTitle ?tokenLabel
```
