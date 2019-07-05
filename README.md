# Mapping Violence Technical Documention

A detailed overview of the code supporting the Mapping Violence project along with a history of changes.

## About

"(Mapping Violence)[http://mappingviolence.com] recovers lost and obscured cases of racial violence in Texas from 1900 to 1930, a period of now largely forgotten bloodshed. Making these histories public is a crucial step towards reckoning with the long legacies of violence."

In support of this effort, the team has undertaken several technical efforts. This includes a managed database, data collection portal, and interactive visualizations. The source for these projects are open source and attempt interventions not only to support Mapping Violence but serve as interventions to the larger discipline of digital humanities and critical race coding.

## History

The first digital archive used by Mapping Violence was a spreadsheet recording high-level metadata and a few sentences of historical information about the incidents of violence. As the number of incidents grew and the information collected become more detailed, maintaining the spreadsheet became difficult. More importantly, the spreadsheet did not offer a successful way to find patterns and trends to suggest areas for additional research. The Mapping Violence team decided a more dedicated solution was necessary.

### Pre-2016 UTRA Efforts

The first software engineer collaborating on the project was [Cailyn Hansen](http://cailynhansen.com). She began in the Spring of 2016 and created the first custom solution that would be partially employed by future efforts at the beginning of the summer.

The software was built on Google App Engine, which is powered by the Java runtime and Java Server Pages (JSPs). It provided a mostly complete interface for entering and storing data. A major limitation (which cannot be remembered presently) prevent the project from fully launching, requiring a migration to a new system architecture.

### 2016 UTRA Efforts

The second custom solution was undertaken in the summer of 2016 by the students and supporting staff and faculty as part of a Undergraduate Teaching and Research Award. This was a natural extension of Cailyn's effort preceding the summer. The development work was again inspired by Java-built web projects, the MVC model of web development, and RESTful APIs. The application was divided into parts: an data entry collection application built using Apache Tomcat and Java Server Pages (JSPs) and a limited functionality REST API built using SparkJava. In addition to Cailyn, [Eddie Jiao]() joined the development team.

#### Data Entry Form

The data entry form began by using most of the code that existed in the pre-summer 2016 project mentioned above. It provided a central dashboard, database overview, individual entry pages, edit pages, and a team directory. Each of these pages were a separate JSP file that included jQuery and custom Javascript to enable interactivity.

#### API

The API provided several endpoints that exposed the entire dataset, individual points of interest, updating points of interest, and changing status (i.e. moving from draft to team to published states). Further, there was the ability to filter fields from the returned results and cursory searching features.

The API was written in Java and the web server was provided by the SparkJava library.

#### Protoype Visualizations

Near the end of the summer, some initial protoypes of potential visualizations were created. [Ben Vars]() was an additional collaborator during this time. The data for the visualizations was supplied by an API mock (as opposed to live-data). Leaflet.js provided the mapping software library for the interactive map. Ben and Eddie worked with sketch-up software to provide examples of tours - that is, curated narratives of a series of incidents of violence.

#### Database

[https://docs.google.com/document/d/1K2-MLniInJklptPMpdp0d7iERMVAazClUIbnulsPLHA/edit](https://docs.google.com/document/d/1K2-MLniInJklptPMpdp0d7iERMVAazClUIbnulsPLHA/edit) describes in detail the data architecture for the project as of 2016's development work.

### 2019 CDS Efforts

Beginning in 2019, the project became part of the Center of Digital Scholarship's (CDS) project portfolio. As part of the migration of project duties, the core team and the CDS team had conversations about the limitations and next steps of the software. JSPs and Apache Tomcat by then had fallen into mostly obsolete territory. Further, CDS did not support Java as a language given incompatible skill sets. 

## Current

The newest version of the software has been written in contempory software and follows best practices in web development and database management. It draws heavily on REST inspired architectures, decoupling of the view from the server business logic, and management of the client state using Flux. In addition, it, like its predecessor, uses a NoSQL database.

### API

There is currently a MVP (minimum viable product) version of an API for the project. It provides an endpoint for the entire dataset, individual entries, creating new entries, updating current entries deleting a specific entry, and deleting all entries. This last endpoint will be removed soon.

The API is written in Javascript and Express.js and run on the Node.js platform.

It is planned for an Apache Solr instance to sit in front of the database and serve as the primary API for searching and filtering the data.

### Database

The database is a document-based, NoSQL storage environment. Specifically, the database is a MongoDB instance. The data storage is divided into three collections: users, entries, and entry_versions. The users collection manages login details for the researchers. The entries collection stores the current version of each of the entries. The only metadata maintained is the ID of the last user to edit the entry and a timestamp for when it was last edited. The entry_versions collection stores all previous version of each entry, including editors and timestamps of the edits. Each document represents a previous (or current) version of an entry and includes a version number.

### Web Interface

The web interface draws heavy design inspiration from the 2016 web interface and follows the material design philosophy. There is a dashboard that lists all current entries, an entry view page,and an entry edit page.

The edit page is powered by JSONForms - a library by EclipseSoftware - that provides the ability to generate forms on the client side as specified by a JSONSchema document and corresponding UI Schema document. This integrates with the rest of the system with React.js and Redux.js. Async store updates are implemented with Redux Saga. 


Disclaimer: Presently the front-end provides no indicator of another user editing a specific entry. This opens the potential for data corruption, especially for long-running edits, if users try to edit the same entry. This limitation has not yet been triaged so a specific timeline for resolving this is not available presently.

## Connection Information

[https://docs.google.com/document/d/1UpumoXwRv0X8urcc2V8TqeSUuQzMoHpZQgOyaB5vhyE/edit](https://docs.google.com/document/d/1UpumoXwRv0X8urcc2V8TqeSUuQzMoHpZQgOyaB5vhyE/edit) provides a detailed description of how to connect to the services mentioned.