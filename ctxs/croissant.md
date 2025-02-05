The MLCommons Croissant Format is a standardized metadata schema designed to enhance the discoverability, portability, and interoperability of machine learning (ML) datasets. Developed collaboratively by industry leaders and academic institutions, Croissant provides a structured way to describe and organize datasets, facilitating their seamless integration across various ML tools and frameworks.

Building upon Schema.org’s Dataset vocabulary—a widely adopted standard for representing datasets on the web—Croissant extends this foundation by incorporating ML-specific metadata. This extension includes details about data resources, structure, and semantics pertinent to ML applications, thereby addressing the unique requirements of ML practitioners.

Google Dataset Search has integrated support for the Croissant format, enabling users to filter search results specifically for Croissant-described datasets. This integration enhances the discoverability of ML-ready datasets across the web.

Leading ML dataset repositories have adopted the Croissant format:

Hugging Face Datasets: Incorporates Croissant metadata into their datasets, facilitating standardized dataset descriptions.
Kaggle Datasets: Provides Croissant-formatted metadata for their datasets, promoting consistency and ease of use.
OpenML: Supports the Croissant format, enhancing dataset interoperability and discoverability.
These adoptions by major repositories and search platforms underscore Croissant's role in streamlining dataset management and utilization within the ML community.

The Croissant metadata is encoded in JSON-LD and is divided into four layers:

Dataset Metadata Layer: Contains general information about the dataset, such as its name, description, license, and URL.
Resource Layer: Describes the source data used in the dataset, including individual files and collections of files.
Structure Layer: Details how the raw data is organized into data structures for use.
Semantic Layer: Provides ML-specific data interpretation and semantics.
Here is an example of a Croissant JSON-LD structure:

{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/",
    "dct": "http://purl.org/dc/terms/",
    "sc": "https://schema.org/"
  },
  "@type": "sc:Dataset",
  "name": "Example Dataset",
  "description": "This is an example dataset in Croissant format.",
  "license": "https://creativecommons.org/licenses/by/4.0/",
  "url": "https://example.com/dataset",
  "distribution": [
    {
      "@type": "cr:FileObject",
      "@id": "example.csv",
      "name": "example.csv",
      "contentUrl": "https://example.com/data/example.csv",
      "encodingFormat": "text/csv",
      "sha256": "examplechecksum"
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "example_records",
      "description": "Records extracted from the example dataset.",
      "field": [
        {
          "@type": "cr:Field",
          "name": "id",
          "description": "The unique identifier for each record.",
          "dataType": "sc:Text",
          "source": {
            "fileObject": { "@id": "example.csv" },
            "extract": { "column": "id" }
          }
        },
        {
          "@type": "cr:Field",
          "name": "value",
          "description": "The value associated with each record.",
          "dataType": "sc:Number",
          "source": {
            "fileObject": { "@id": "example.csv" },
            "extract": { "column": "value" }
          }
        }
      ]
    }
  ]
}
In this example, the JSON-LD structure provides metadata about the dataset, describes the distribution of the data files, and defines the structure of the records within the dataset. This standardized format facilitates the integration and use of the dataset across various ML frameworks and tools.
