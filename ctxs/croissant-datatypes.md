MLCommons Croissant defines its own vocabulary in the namespace:
	•	Namespace & Prefixes
	•	Croissant Vocabulary: Defined in its own namespace identified by the IRI http://mlcommons.org/croissant/, generally abbreviated as cr.
	•	Additional Namespaces: Examples include:
	•	sc: for schema.org
	•	dct: for Dublin Core Terms (http://purl.org/dc/terms/)
	•	wd: for Wikidata (http://www.wikidata.org/wiki/)

Since Croissant builds on the widely adopted schema.org vocabulary, examples typically use schema.org as the default namespace, while Croissant-specific terms are prefixed with cr. A JSON‑LD context is used to define aliases so that explicitly specifying prefixes is not always necessary.

Croissant uses standard JSON‑LD mechanisms to connect various elements within a dataset:
	•	Identifiers (@id):
	•	Every significant element (like FileObject, FileSet, or RecordSet) must have a unique identifier defined using the @id property.
	•	IDs can be short strings but are interpreted as IRIs. The “base” IRI is either the document’s URL (when accessed online) or explicitly set using the @base property in the JSON‑LD context.
	•	For nested objects (e.g., fields inside a RecordSet), it is recommended to prefix the nested ID with the parent’s ID and separate them with a /.
Example: A field named “date taken” inside an “images” record set might have the ID images/date_taken.
	•	JSON‑LD properties (those starting with @) form part of the RDF syntax used in Croissant.
	•	A field may have multiple assigned data types, with at least one being an atomic type (e.g., sc:Text) to ensure basic compatibility, while additional types can provide richer semantic context.
	•	Common atomic data types come from schema.org, such as sc:Date (from schema.org/Date).
Croissant allows the integration of data types from external vocabularies like Wikidata:
	•	Semantic Enrichment:
	•	For example, using wd:Q48277 (a Wikidata identifier for gender) can indicate that a particular Field or RecordSet contains values that reflect someone’s gender.
	•	This mechanism supports advanced applications, such as Responsible AI (RAI) frameworks, which might flag potential biases by associating values with specific identifiers (e.g., wd:Q6581097, wd:Q6581072).
	•	Use Case Example:
	•	A field labeled url might be expected to contain URLs that refer to cities. By assigning it a semantic type from Wikidata (e.g., wd:Q515 for the City class), users are led to expect URLs that reference specific city entries (such as https://www.wikidata.org/wiki/Q90).
