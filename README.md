VIP Flat File Data
==============
## For States ##
This repo contains five directories, which you can use to build your flat files for the Voting Information Project. 

## Version 5.1 ##
The `vip_5.1_csv_templates` folder contains the structure of each type of file that can be used to publish VIP data in the 5.0 specification. The `vip_5.1_csv_examples` folder contains sample data. The `VIP_51_diagram.svg` file shows a graphical mapping of the 5.1 CSV objects. 

## Individual file notes ##

Use the file templates provided in the `vip_5.1_csv_templates` folder

### These files must be included in every set of data files ###

* **`election.txt`** &mdash; if you have more than one absentee request deadline, use the earlier of the two 
* **`source.txt`**&mdash; most often, the VIP id will be a state's FIPS code
* **`state.txt`** 
	
### These files will be included in your standard data file set (in addition to the required files above)###

* **`election_administration.txt`** &mdash; the `eo_id` column should correspond to the ids in the `election_official.txt` file
* **`locality.txt`** &mdash; this is the district directly below state, most often the county. Ensure the `state_id` column matches the id in the `state.txt` file and the `election_administration_id` column matches the id from the `election_administration.txt` file.
* **`polling_location.txt`** &mdash; we understand that most `polling_location.txt` files will change, even after the locations are set in-state
* **`precinct.txt`**
* **`street_segment.txt`** &mdash; these must not overlap, or there will be errors when compiling the data into XML. We will provide you with the IDs of the overlapping segments, if your file contains them.

### These files will need to be included to display contest data for candidate contests###

* **`contest.txt`** &mdash;
* **`candidate.txt`** &mdash;
* **`candidate_contest.txt`** &mdash;
* **`candidate_selection.txt`** &mdash;
* **`ballot_selection.txt`** &mdash;
* **`office.txt`** &mdash;

### These additonal files will need to be included to display contest data for referenda/propositions/ballot measures###

* **`ballot_measure_contest.txt`** &mdash;
* **`ballot_measure_selection.txt`** &mdash;

### These additonal files will need to be included to display contest data for retention contests###

* **`retention_contest.txt`** &mdash;



## Version 3.0 ##
The `data_templates` folder contains the structure of each type of file that can be used in publishing VIP data. The `example_data` folder contains portions of the data files from NC, so you can look at an example of actual data. More information about the different VIP files is below.

## About files in data_templates ##

It is a good idea to read through the XML specification [located here](http://votinginfoproject.github.io/vip-specification/). It will give you a good idea of what your data will look like after the flat files are compiled into an XML file. In order for the data to compile correctly, the format of the flat files must match the templates exactly - identical layout and format. 

A few notes about the flat files before you begin:

* The XML specification states that each ID must be unique in a data file. Here, it is referring to an entire XML file, not an individual flat file. What that means is that every flat file you produce must have IDs that do not match IDs in any other file. For example, you cannot number your locality ID as 1-50 and your `election_official` ID as 1-50. To prevent this, we suggest pre-pending additional digits to ID fields that are not unique. For example, adding '222000' to the ID in `polling_locations.txt` and '333000' to the ID in precincts, etc. You can do this while running the data query by using a concatenate function. For example, in an Oracle database:

    ```sql
    SELECT '22000' || id AS "id",
    ...
	```

* Each file must be a comma delimited, UTF-8 file, preferably a .txt file. We can also convert .csv files to .txt. In addition to being UTF-8, the files should not contain non-ASCII characters.
 
* Each file should be named exactly as the files are named in `data_templates`.
 
* The columns must match the column names and order of the examples in `data_templates`. It is usually ok to leave a column blank if you do not have that data, i.e. `candidate_url`. If you have questions about whether a column is required or not, refer to the XML specification for that element: http://votinginfoproject.github.io/vip-specification/

* When exporting your files, do not replace blank or empty fields with NULL.

* Your data files should consist only of files that exist in the data_templates folder. You don't need to have every one of the files, but extra files will not work with the processor we have to compile the XML.

## Individual file notes ##

Use the file templates provided in the `db_templates` folder

### These files must be included in every set of data files ###

* **`election.txt`** &mdash; if you have more than one absentee request deadline, use the earlier of the two 
* **`source.txt`** &mdash; most often, the VIP id will be a state's FIPS code
	
### These files will be included in your standard data file set (in addition to the required files above)###

* **`election_administration.txt`** &mdash; the `eo_id` column should correspond to the ids in the `election_official.txt` file
* **`election_official.txt`**
* **`locality.txt`** &mdash; this is the district directly below state, most often the county. Ensure the `state_id` column matches the id in the `state.txt` file and the `election_administration_id` column matches the id from the `election_administration.txt` file.
* **`polling_location.txt`** &mdash; we understand that most `polling_location.txt` files will change, even after the locations are set in-state
* **`precinct.txt`**
* **`precinct_polling_location.txt`**
* **`state.txt`** 
* **`street_segment.txt`** &mdash; these must not overlap, or there will be errors when compiling the data into XML. We will provide you with the IDs of the overlapping segments, if your file contains them.

### Include these files if you are including data for candidates on the ballot###

* **`candidate.txt`** &mdash; the column `sort_order` refers to the candidate's order on the ballot option
* **`contest.txt`** &mdash; ensure that the second column, `electoral_district_id`, matches up to the ids in the `electoral_district.txt` file. If your data set contains ballot or candidate information, ensure that the column `ballot_id` corresponds to the IDs in the 'ballot.txt' file.
* **`ballot.txt`** &mdash; use this file to represent ballots
* **`ballot_candidate.txt`** &mdash; use this file if you have candidate data for your ballots
* **`electoral_district.txt`** &mdash; this contains any electoral district that you will have contest or ballot information for. The `number` column must contain only numbers.
* **`precinct_electoral_district.txt`** &mdash; this file will usually have many records for each precinct with the id of each electoral district it falls under

### Include these files if you are including referendum data (in addition to the files necessary to publish candidate data)###

* **`ballot_response.txt`** &mdash; this file details all possible responses to a contest in a ballot
* **`custom_ballot.txt`** &mdash; only include this file if you have custom ballot data
* **`custom_ballot_ballot_response.txt`** &mdash; only include this file if you have custom ballot data
* **`referendum.txt`** &mdash; this file contains the text of the referenda for the election
* **`referendum_ballot_response.txt`** &mdash; this file links referenda responses back to the ballot and ballot response files

### Include these files if you have early vote site data ###

* **`early_vote_site.txt`**
* **`precinct_early_vote_site.txt`**
* **`locality_early_vote_site.txt`** &mdash; you can use this file to link districts and early vote sites, if this is not done by precinct in your state
* **`state_early_vote_site.txt`** &mdash; you can use this file to link state and early vote sites, if this is not done by precinct in your state

### Include these files if you have precincts that are split between electoral districts ###

* **`precinct_split.txt`**
* **`precinct_split_electoral_district.txt`** &mdash; like the `precinct_electoral_district.txt` file, this will usually have many records for each split with the id of each electoral district it falls under
* **`precinct_split_polling_location.txt`** &mdash; use this file if polling locations are assigned by precinct split in your state
