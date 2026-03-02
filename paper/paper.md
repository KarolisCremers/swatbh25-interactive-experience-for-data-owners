---
title: 'BioHackSWAT4HCLS25 report: Towards an interactive mapping experience for data owners'
title_short: 'BioHackSWAT4HCLS25 #26: Improving RDFCraft'
tags:
  - user experience
  - RDFCraft
  - SHACL
  - Docker
authors:
  - name: Karolis Cremers
    orcid: 0000-0002-1756-3905
    affiliation: 1
  - name: Daphne Wijnbergen
    orcid: 0000-0002-7449-6657
    affiliation: 1
  - name: Julia Koblitz
    orcid: 0000-0002-7260-2129
    affiliation: 2
  - name: Javier Millan Acosta
    affiliation: 3
    orcid: 0000-0002-4166-7093
  - name: Rick Overkleeft
    orcid: 0009-0004-3529-1159
    affiliation: 1, 4
  - name: David Schimmel
    orcid: 0000-0002-1719-8928
    affiliation: 5
  - name: Katja Hoffmann
    orcid: 0000-0003-4765-0767
    affiliation: 6
  - name: Brieuc Quemeneur
    orcid: 0009-0006-7574-2254
    affiliation: 7
  - name: Ensar Emir Erol
    orcid: 0000-0002-5739-8860
    affiliation: 8



affiliations:
  - name: Leiden University Medical Center
    ror: 05xvt9f17
    index: 1
  - name: Leibniz Institute DSMZ - German Collection of Microorganisms and Cell Cultures, Germany
    ror: 02tyer376
    index: 2
  - name: Translational Genomics, Maastricht University
    index: 3
  - name: 4MedBox, Leiden
    index: 4
  - name: IT Center, RWTH Aachen University
    ror: 04xfq0f34
    index: 5
  - name: Institute for Medical Informatics and Biometry, Faculty of Medicine Carl Gustav Carus, TUD Dresden University of Technology, Germany
    index: 6
  - name: Institut du Thorax - UMR1087, Nantes, France
    index: 7
  - name: Institute of Data Science, Maastricht University
    index: 8
  
date: 27 February 2025
cito-bibliography: paper.bib
event: BH25SWAT4HCLS
biohackathon_name: "BioHackathon SWAT4HCLS 2025"
biohackathon_url:   "[https://www.swat4ls.org/](https://www.swat4ls.org/workshops/barcelona2025/programme/swat4hcls-2025-biohackathon/)"
biohackathon_location: "Barcelona, Spain, 2025"
group: Project 26
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/KarolisCremers/swatbh25-interactive-experience-for-data-owners
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Karolis Cremers, Daphne Wijnbergen, \emph{et al.}
---


# Introduction
At the Barcelona SWAT4HCLS 2025 Hackathon, familiarize with and worked on improvements for RDFCraft.

## Background
Mapping data to an implicit or explicit schema to onboard data unto a registry, database or analysis pipeline is a universal experience for data focused researchers. In the field of semantic web application and tools metadata are often structured in the Resource Description Framework (RDF) [@RDF] and described through ontologies [@ontologies]. These highly structured descriptions allow for machine readability and automation [@thirumahal2022semantic]. Mapping data to a metadata schema often requires extensive knowledge of the data, the model and familiarity with a programming or mapping language such as Resource Mapping Language (RML)[@dimou2014rml] or YARRRML[@yarrrml].

Data owners without technical know-how of semantic technologies and programming have an especially high learning curve to semantically represent their data. To combat this learning curve, tools that reduce the technical load, like RDFCraft, are developed. [RDFCraft](https://github.com/MaastrichtU-IDS/RDFCraft) is a data onboarding tool that helps with mapping tabular or JSON formatted data to a reference schema ontology. The tool consists of a data parser, an ontology parser and GUI that allows for semi-automatic mapping of data to ontology terms. The tool outputs both an RML file and RDF representation of the data described with the provided ontology. Through this functionality data owners do not need extensive knowledge of RML or RDF and can assign data attributes to their ontology terms in a more streamlined manner.

## Hackathon objectives
The version of RDFCraft at time of the hackathon required the user to type in the matching ontology term to the data attribute. Even with autocomplete function, semi-automatic functions and LLM integration this requires many written user inputs. A proposed solution to this is the use of a drag-and-drop system where users can drag a representation of the data attribute on top of the associated ontology term in a visualized schema to perform the mapping.

To visualize the schema automatically by the tool it should be able to identify and differentiate between classes and properties.

The original objective of this hackathon was to enhance the RDFCraft tool to improve the interactive experience for data owners. Specifically, we aimed to: Parse SHACL (or Shex or alternative) shapes to extract different classes necessary for schema compliance validation; Automatically visualize the classes in a graph and implement SHACL (or ontology) validation to mappings.

At the beginning of the session we defined the following subgoals with participants:

### Backend Enhancements
Improving the backend functionality would open up opportunities for development of new features and automated processes in the tool. We aimed to set up a database module to store the schemas and input data for stable and fast access.


#### Format Parsing
(Meta-)Data schemas are often written in different formats, as such we aimed to parse both OWL (Web Ontology Language) and SHACL (Shapes Constraint Language) representations into classes that could be visualized in the UI components. SHACL files are especially valuable, as these could be used to validate the mappings based on the values of the data attribute.
Additionally, participants of the hackathon agreed that including additional input file formats would increase the value of the tool within their respective work environment. As such, an additional goal was set to develop a module to parse VCF (Variant Call Format) files within the tool.


### UI Enhancements
To enable drag-and-drop functionality, in a convenient manner, there are several additions needed to the UI: First, automatic generation of initial nodes classes extracted from the schema(s) that provide targets to drop data attributes on. Second, add a drag-and-drop interaction from the “references” tab to graph node/property box that triggers a mapping event. Implement or reuse an event that adds an attribute path (location in the given data file) to the node or property within the RML. 
Represent SHACL validation errors in the UI. Finally, a SHACL option should be added to the "add ontology" window; 


# Results

### Docker Enhancements
Based on our experimentation during the hackathon enhancements have been [implemented into the tool](https://github.com/MaastrichtU-IDS/RDFCraft/pull/40).

**Dockerfile Enhancements**:  
We encountered problems with dependencies during experimentation with the API. Dependencies were written in separated containers using Docker and Docker Compose to avoid requiring Java installation on the local machine.

**Docker Documentation**:  
To make sure the docker implementation API works for everyone, the necessary actions have been added to the documentation:
```
* Expose port 8080 or desired port in the Dockerfile
* Add the port argument `-p 8080:8000` to map internal port 8000 to external port 8080 or desired port. 
* Connect to `http://localhost:8080/` or `http://localhost:PORT/`.
```

### Format parsing
The development of SHACL parsing in the tool was limited by Docker issues. Extraction of properties from SHACL was succesful, however, integration of the classes into the tool was not achieved by the end of the hackathon.


### UI Enhancements
The following two UI enhancements have been [added to the tool](https://github.com/MaastrichtU-IDS/RDFCraft/pull/39):  
We added a quality of life change to the behavior of the UI: After adding a node, it is automatically selected. Additionally, Nodes of different types (entity, literal, URI) are colored differently to indicate their differences. These colors correspond to the colors given to the node types in the overview window was already present in the UI.

Finally, in preparation for the automatic visualization of schema classes in a graph, a [Python function](https://github.com/dwijnbergen/Node_functions/blob/main/estimate_node_height.py) was created to estimate node height. When a node graph is generated automatically, it is important to position the nodes without them overlapping. This function follows the following pattern:   
``` 
Handle height = 10 px.
Title height = 18 px * [number of lines] + 35 px.  
Property height \approx 50 px * [number of properties].
```


# Conclusion
The work our group has done provides a foundation for further improvement of the RDFCraft tool.
A [GitHub Fork](https://github.com/jmillanacosta/RDFCraft/tree/shapes) was created where improvements to the tool were pushed. These improvements published on the fork have been [integrated into the original RDFCraft tool GitHub](https://github.com/MaastrichtU-IDS/RDFCraft/pull/39) by the original creator of RDFCraft. First, the Docker implementation was improved for future developments. Second, Quality of life changes in the behavior of the UI and visual clarity improvements. Finally, we took the first steps towards new features by developing algorithms for node box sizes and positions. 

# Jupyter notebooks, GitHub repositories and data repositories
GitHub Fork: https://github.com/jmillanacosta/RDFCraft/tree/shapes  
original RDFCraft tool github: https://github.com/MaastrichtU-IDS/RDFCraft/commit/d6a7623edcc2cf677292fd9c611e6694680e4d19  

# Future work
One of the main barriers for enabling community development of a tool is its ease of installation. Improvement on this aspect will promote collaboration with peers. To ensure the process is not overwhelming for users unfamiliar with mapping we recommend: Initially only showing mandatory classes and their mandatory properties, sorted alphabetically. Additionally, the schema graph should progressively expand based on filled-in mappings to promote mapping completeness. Finally, expand/collapse functionality of nodes would give control to the user on the extend of the schema shown.


## Acknowledgements
Many thanks to BioHackathon SWAT4HCLS 2025 for organizing the event, work space, food and drinks during the hackathon.

## References

