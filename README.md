# WMU Research Topic Visualization Project
Daria Orlowska, Data librarian and Assistant Professor at Western Michigan University <br>
Last updated: 2024-04-22

# Purpose
The purpose of this project is to identify what research is happening across campus and facillitate more interdisciplinary collaboration by making matches based on research topics. The project originated in August 2019, and sought to capture research tenure-track faculty were engaging in primarily through an API Scopus pull. After discussing the projecw the University Libraries colleagues, the Office of Research and Innovation as well as with Associate Deans for Research, it was determined that using Scopus API is a good start but does not capture researchers on campus who are not participating in "traditional" research scholarship or who do not have articles indexed by Scopus. This current project attempts to build on the previous interation in several ways:
* Expanding the definition of researcher beyond tenure-track faculty, by including anyone who might be producing scholarship
* Expanding the researcher dataset to include Wmed faculty, who are looking to increase collaboration with WMU
* Expanding inclusions of researchers with scholarship not indexed by Scopus, but including user-generated research interests from a variety of sources

Project output include three main files, which in conjunction can be used to facilitate conversation about possible research collaborators:
* An Excel spreadsheet containing information about the researcher (researcherID, name, alternate name, title, affiliation, ORCiD ID, ORCiD profile metadata, ScopusID(s), directory metadata)
* A csv obtained from the Scopus API pull containing all scholarship indexed by Scopus for each researcher with a ScopusID (along with the scholarship metadata)
* A csv created by the python script in this repository which aggregates researchers by research keyword

# Researcher Dataset Aggregation

## Researcher list aggregatation
Conducted on: January 2024 <br>
The Excel spreadsheet of tenure-track faculty and their affiliations generated in 2019 was used as the basis for the expanded researcher dataset. For more information about how this initial spreadsheet was generated, please see the previous version of this README.

### Additional researcher sources
Due to the difficulties in obtaining a list of all faculty and researcher on campus, the original dataset was expanded by incorporating the following sources:
* A pull from Alma, a library management system that includes classification of users by type but does not include any additional user information. Only individuals with the classification "faculty" were retained
* A pull from Cognos, a university institutional data reporting system that includes titles but only provides information on instructors teaching in the past year. Only individuals with faculty classification were retained (assistant, associate, professor, part-time, specialist, etc)
* A pull from FARS, the faculty annual reporting system containing faculty research interests from the years 2019 and 2024, obtained from Institutional Research
* A spreadsheet of new hires, obtained from the 2023 new faculty orientation
* A spreadsheet of researchers involved in grant submissions over the past 6 years, obtained from the Office of Research and Innovation
* A spreadsheet of Wmed faculty, obtained from the grants coordinator at Wmed
* A faculty catalog from 2021, obtained from the WMU website
* The Board of Trustees (BoT) minutes from the past 3 years, obtained from ScholarWorks, WMU's institutional repository
* A list of research associates and deans, obtained from the Outlook directory

### Corrections and exclusion criteria
* Names were de-duplicated, with researchers with multiple affiliations (departmental and university) being represented as one entry
* Individuals not found in the People Search (university directory), or who were listed as students or provisionary employees without a title were excluded from the list
* Names were devoid of special characters and contained the most complete version of the name, obtained from the People Search, ORCiD directory, or publication name in Scopus
* Nicknames, alternate names, and special characters were included in a separate column
* In the case where departmental affiliation or title were in any of the sources, a search was conducted within the university directory to obtain this extra information

## Additional research keyword sources
Conducted: April 2024
In an effort to include more researchers in keyword matching within the python script pulling from the Scopus API, additional keywords were aggregated from the following sources:
* Bulleted research interests or topics listed within the biography within researcher WMU directory pages
* Bulleted research or topics listed within the biosketch within researcher Wmed diretory pages
* Keywords provided within ORCiD profiles
* Keywords provided by WMU faculty as part of FARS in 2019/2024
* Keywords provided within aggregate WMU department research websites

## ORCiD profile matching
Conducted: February 2024 <br>
### Matching faculty names to profiles in ORCiD
* used ORCiD search to check all faculty entries against ORCiD database: https://orcid.org/orcid-search/search
* match with slightly different name was confirmed with faculty profile on WMU, and name in list was modified
* ORCiD numbers were recorded
* ORCiD metadata in researcher profile was recorded (employment, linked profiles, number of works listed, keywords provided) 

### ORCiD profiles were encoded as follows:
* confirmed: url to ORCiD profile
* unconfirmed: if records came up but it was not clear if they belonged to the WMU/Wmed researcher (and further coded as "one", "some", and "many" to indicate amount of unconfirmed results)
* no results: no results found | all results found were definitely not Western faculty (wrong field, recently updated profile with different university affiliation)
* employment: title of latest employment location entered, or entered "none" if no employment was listed
	
## Scopus (Elsevier) profile matching
Conducted: March 2024 <br>
A number of researcher entries already contained ScopusIDs from the 2019 search (see previous version of this README to see how that was conducted), or through ScopusIDs provided as part of the ORCiD profile. Any researcher without a ScopusID was looked up manually by searching for first and last name within the Scopus Search. A download of all WMU-affiliated researchers within Scopus could be conducted, and those entries could be matched with the researcher dataset, but this would omit researchers who have not published since starting employment with WMU, researchers with multiple ScopusIDs, and could potentially result in a mismatch for researchers with similar names.

# Preparing Script Files
Five plain text (txt) files are necessary to run the python script included in this repository, including a comma separated value (csv) file generated as part of the script:
* <i>ScopusID_complete-list.txt</i>, which is a list of all the research ScopusIDs, separated by commas
* <i>ScopusID_WMU-list.txt</i>, which is a list of only WMU reearcher ScopusIDs (excluding Wmed researchers), separated by commas <-- this can be excluded if the related sections are commented out
* <i>2024-04-17_research-articles-complete.csv</i>, which is a spreadsheet generated by the scripting listing the metadata of all the scholarship generated by the ScopusIDs in the file ScopusID_complete-list.txt
* <i>researcherID_scopusID.txt</i>, which is a list of unique IDs assigned to each researcher connected to their ScopusID(s), in the format researcherID:ScopusID1; ScopusID2
* <i>researcherID_additionalkeywords.txt</i>, which is a list of unique IDs assigned to each researcher connected to a concatenated list of additional research keywords (ORCiD, FARS, directory, aggregate website) in the format researcherID:keyword1; keyword2
* <i>researcherID_researcherdept.txt</i>, which is a list of unique IDs assigned to each researcher connected to their publication name and affiliated departments in the format researcherID:name;department1 | department2

# Running Python Script
The python script was prepared in python 3.9, using Jupyter notebooks as part of the Anaconda navigator. To run the script, place the script and the underlying files within the same folder. Then update the file names within the script, if necessary. For first time script runners, some python libraries will need to be installed prior to running using the pip installer (such as pybliometrics and pandas). Accessing the Scopus API requries obtaining a Scopus API key and being on the campus network to run a pull-- additional information is included within the jupyter notebook documentation. After all of these items are setup, the user can choose "Run All" to execute the entire script and receive all the file outputs, or just part of the script in case they just want to generate a Scopus scholarship pull. 
