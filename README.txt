Documentation for the WMU Research Topic Visualization project
Created by: Daria Orlowska
Last updated: 2019-12-12

# Department faculty pull from directory
Conducted on: 2019-08-12
Each department was individually accessed through the website https://wmich.edu/directories/departments/

## The following xpath queries were used in the browser add-on Xpath Helper to pull names and titles:
	* Name: //div[@class='views-field views-field-title']/span[@class='field-content']
	* Title: //div[@class='views-field views-field-field-official-title']/div[@class='field-content']

## The contents of the pulls were pasted into an excel spreadsheet titled "Directory Pull RAW" and lightly cleaned:
	* Multiple spaces were deleted
	* Periods were deleted
	* Titles were added manually using the WMU People Search if missing: https://wmich.edu/peoplesearch/

## The "Directory Pull RAW" was then copied over to a new sheet, where only faculty were retained:
	* Any title containing "professor" [N=774]
		--> conditional formatting was used to highlight this keyword, data was sorted by conditional formatting, remainder was deleted
	* Any title containing "researcher" [N=0]
		--> none were found that weren't already captured by the keyword "professor"
	* Any title containing "chair" [N=49]
		--> conditional formatting was used to highlight this keyword, data was sorted by conditional formatting, remainder was deleted

## Faculty-only list was looked over and the following was corrected:
	* Spaces were introduced in names where FirstLast and FirstMiddle occurred
	* Nicknames were replaced with full first names
		--> name changes were highlighted in yellow
	* Names were de-duplicated
	
# New hire additions
Conducted on: 2019-08-22
Used pdf of new faculty orientation list to supplement list from directory
	--> omitted instructors, those without a title, and faculty specialists
Also eliminated any individuals with visiting faculty or faculty specialist titles from main list at this time
	--> due to lack of research or main affiliation to WMU


<The following were additional processes that were undertaken but can be skipped>
--
# Matching to ORCiD
Conducted: 2019-08-14 through 2019-08-22
Matching faculty names to profiles in ORCiD
	--> match with slightly different name was confirmed with faculty profile on WMU, and name in list was modified
	--> ORCiD numbers were recorded
	--> used ORCiD search to check all faculty entries against ORCiD database: https://orcid.org/orcid-search/search

## Profiles were encoded as follows:
	* unconfirmed: one record, no information (record update date recorded)
	* unconfirmed many:	many records without public information (manual investigation if less than 10 records)
	* no results: no results found | all results found were definitely not Western faculty (wrong field, recently updated profile with different university affiliation)
	* affiliation update: Y if listed affiliation in profile | N if did not list affiliation but mentioned Western Michigan in biography, linked to faculty page, or was able to link based on affiliation in listed articles
	
# Matching to Publon by institution
Conducted: 2019-08-22
Matching faculty names to profiles in Publons (Web of Science): https://publons.com/institution/1024/
	--> match with slightly different name was confirmed with faculty profile on WMU, and name in list was modified
	--> Publon number was recorded
	--> ORCiD number was recorded if Publon allowed for connection to be made

Checked list with all four results (#: 332978, 91905, 1024, 59717)
	--> initially cycled through people listed as affiliated to WMU in those lists
	--> later used the browse researchers tab to try to get more matches: https://publons.com/researcher/?is_core_collection=1&order_by=verified_reviews_performed_last_12_months
---


# Matching to Scopus (Elsevier)
---
<Intial attempt>
## Affiliated via preview
Conducted: 2019-08-23
Filtered scopus by institution (search query to the right), viewed all authors, checked list with last names & selected "show preview" using arrow next to number of publications to access author details (profile)
	--> AF-ID ( "Western Michigan University"   60000879 )  OR  AF-ID ( "Western Michigan University Battle Creek"   60028175 )  OR  AF-ID ( "Western Michigan University Grand Rapids"   60012902 )  OR  AF-ID ( "Western Michigan University Holland"   60002996 )  OR  AF-ID ( "Western Michigan University Homer Stryker M.D. School of Medicine"   60113710 )  OR  AF-ID ( "Western Michigan University Lansing"   60015211 )  OR  AF-ID ( "Western Michigan University Muskegon"   60003101 )  OR  ( ... )  OR  AF-ID ( "Western Michigan University Traverse City"   60021571 )
	--> cycled through all affiliated profiles on first page of preview (didn't realize there were more pages)

Extracted profile information using Xpath Helper
	--> Topics (bottom tab) extracted with query: //table[@id='topics-table']/tbody//td[1]//span[@class='anchorText']
	--> Subjects (floating) if length, extracted with query to string together with regex: //div[@id='subjectAreaBadges']/span[@class='badges']

## Affiliated via download
Conducted: 2019-09-05
Filtered by affiliation "60000879" and downloaded entire list of authors affiliated with institution [N=4278]
	--> https://www.scopus.com/affil/profile.uri?afid=60000879
		-> clicked on number of authors (hyperlinked)
		-> checked the box next to "All" in the bar above the results, and clicked "export CSV"
	--> used data > text to columns in directory pull spreadsheet to separate out last name from first name
	--> matched last name from directory pull with last name from affiliation download spreadsheet using IF formula:
		=IF(C2="","",IF(COUNTIF('https://wmich-my.sharepoint.com/personal/djd9562_wmich_edu/Documents/Daria Notes/WMU Research Pull/outdated/[2019-08-22 Directory Pull.xlsx]2019-08-12 Faculty List'!$B:$B,C2)=0,"","x"))
		in the Scopus affiliation download spreadsheet, if the last name first is not blank, then check it against the last name in the faculty directory pull spreadsheet
		if you find a last name match, create an x; otherwise, leave the cell blank

Used matches ("x") to quickly grab the <author ID> in the affiliated download spreadsheet to insert into the url: https://www.scopus.com/authid/detail.uri?authorId=<author ID>
	--> extracted topivs and subjects from author profiles with the help of Xpath Helper
			* Topics (bottom tab) extracted with query: //table[@id='topics-table']/tbody//td[1]//span[@class='anchorText']
			* Subjects (floating) if length, extracted with query to string together with regex: //div[@id='subjectAreaBadges']/span[@class='badges']
	--> recorded Scopus author ID (some had multiple)
	--> performed quick search to make sure there were no additional Scopus author ids attached to same author
	--> searched missing authors using author search function: https://www.scopus.com/search/form.uri?zone=TopNavBar&origin=searchbasic&display=basic#author
---

## Suggestion based on attempt (for efficiency)
Filter by affiliation and download entire list of authors affiliated with institution
	--> https://www.scopus.com/affil/profile.uri?afid=60000879
		-> click on number of authors (hyperlinked)
		-> check the box next to "All" in the bar above the results, and clicked "export CSV"
	--> use data > text to columns in directory pull spreadsheet to separate out last name from first name
	--> match last name from directory pull with last name from affiliation download spreadsheet using IF formula:
		=IF(C2="","",IF(COUNTIF('https://wmich-my.sharepoint.com/personal/djd9562_wmich_edu/Documents/Daria Notes/WMU Research Pull/outdated/[2019-08-22 Directory Pull.xlsx]2019-08-12 Faculty List'!$B:$B,C2)=0,"","x"))
		in the Scopus affiliation download spreadsheet, if the last name first is not blank, then check it against the last name in the faculty directory pull spreadsheet
		if you find a last name match, create an x; otherwise, leave the cell blank
Use matches ("x") to quickly grab the <author ID> in the affiliated download spreadsheet to insert into the url: https://www.scopus.com/authid/detail.uri?authorId=<author ID>
	--> record Scopus author ID (some had multiple)
	[OPTIONAL, conduct for completion]
	--> perform quick search to make sure there were no additional Scopus author ids attached to same author
	--> search missing authors using author search function: https://www.scopus.com/search/form.uri?zone=TopNavBar&origin=searchbasic&display=basic#author

## Extract author keywords using Scopus api and author IDs
	--> request api access on Scopus
	--> create a .txt document of author names and their scopus ids
		AuthorFirst AuthorLast:ID1,ID2,ID3
	--> import file into jupyter notebook "ScopusAPI_pull.ipynb", enter authentication key and run
	--> copy or export out results of "ScopusAPI_pull.ipynb" script into a .csv file, with authors in one column and topics in the another

# Preparing for Visualization
Conducted: 2019-11-05
## Clean the author keywords
	--> author keywords are author-generated and not standardized
		-> to create a network visualization with the maximum  amount of links, some cleaning is strongly recommended
	--> then use OpenRefine to import your newly created .csv as a new project
		-> standardize the case (lowercase)
		-> trim whitespaces
		-> use clustering to standarize the keywords until you are content
		NOTE: decide if you want to keep the singular or plural version of each keyword (ex: students or student)
	--> export project into a .csv and save your change log
	--> use Excel to de-duplicate your author:topic entries
	
## Create dictionary of where author keyword is the key, and faculty who are studing a given topic are the values
	--> use the jupyter notebook "Author Keywords to Faculty.ipynb" to import your cleaned .csv file and run to create a spreadsheet of topics, researchers involved in the topic, and their departments

## Create linkages between faculty researching the same topic
	--> manually create matches in the following format: facultyA topic facultyB
		-> make sure that all faculty matches are accounted for
		-> (ex: if there are 5 faculty studing the same topic, 10 matches should be made)
	--> create a spreadsheet where one sheet is faculty links (RELATIONS), and the other is faculty information (name, department, title) (NODES)

# Visualizing your research network in Cytoscape
Conducted: 2019-11-08
## Import spreadsheet
	--> import network from file, choose RELATIONS
		-> source - interaction - target
	--> import table from file, choose NODES
		-> selected networks only
	--> group faculty by departments
		-> layout > group attribute layout > all nodes > type (dept)
		
## Filtering based on researcher
	--> use the bar on the top right to search for researcher (FirstName LastName)
	--> select > nodes > nodes connected by selected edges (ctrl+7)
	--> select > hide interjected nodes and edges
	
## Filtering based on topic
	--> control panel on left hand side
		-> select > edge:interaction contains <topic>
		-> select > nodes > nodes connected by selected edges
		-> select > hide interjected nodes and edges

