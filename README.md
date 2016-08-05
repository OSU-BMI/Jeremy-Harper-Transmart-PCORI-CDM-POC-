Transmart-PCORI-CDM-POC
Loading the PCORI CDM into transmart via a proof of concept ETL

Background: With ~800,000 Individuals and +100million observation facts the existing ETL's did not accomodate our needs. In addition the CDM is marked by having overtime data and the existing ETLs (and transmart) have not been developed to handle those capabilities. We have done a Proof Of Concept (POC) ETL that takes ~8 hours of processing to fully load the databases on our stand alone machine. The machine is a 2012 Macbookpro running ubuntu/postgres. Due to the sensitive nature of the database we opted to run it on a stand alone machine rather than on the network while attempting the POC. 

Overview - Setup

1.	Install transmart https://wiki.transmartfoundation.org/display/transmartwiki/Install+the+current+official+release
2.	Load PCORI database structure into transmart database via SQL script: https://github.com/LHSNet/PCORNet-CDM/blob/master/PCORNet-CDM-v3/sql/pgsql/create_pcori_cdmv3_tables_postgres.sql 
3.	Import PCORI Data from tsv
4.	Run transmart conversion SQL statement script
5.	Run a complete re-index of postgres afterward via unix command line 
6.	Database is available in transmart

Created Transmart tree values to model the following CDM v3 tables
--Demographic --Diagnosis --Death --Prescribing --Condition --Dispensing --Vital Height --Vital Weight --Vital Diastolic --Vital Systolic --Vital Original_BMI --Vital Smoking --Vital Tabacco --Lab_Result_CM --Procedures

Missing tables, issues representing them in the tree
--Encounter --PCORNET_TRIAL --HARVEST --PRO_CM

Example of how our CDM tree appears in transmart
![diagnosis tree with dates obfuscated](https://cloud.githubusercontent.com/assets/16839412/17255910/2398d7a0-5589-11e6-9493-253a5d1406a4.jpg)

Limitations –

1.	We did not attempt to visualize every single data element from PCORI CDM there are optimization opportunities
2.	We did not convert from PCORI CDM internal identifiers to English
3.	This script only works with postgres (Although using purely SQL where possible)
4.	We have found the query visualizer to run very slowly after import but it does render the queries eventually
5.	We may have missed tables that should be populated for better performance
6.	Since this was a stand alone instance we created a public study. You’ll want to modify to private if you are going to do this in a production table. You’ll also need to modify the import scripts as they assume they can create the identifiers. We didn’t find where transmart stores its counters until after we’d implemented.
7.	For some reason age registers but race_cd and sex_cd do not on the patient dimension. You can still have those displayed since they are in the tree as an option but its something to fix.

