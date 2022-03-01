---
layout: post
title:  "Oracle 11g / 12c Tutorial Topics"
categories: netdevops
tags: Network Engineer
date: 2022-3-1
---

Oracle 11g / 12c Tutorial Topics

## Oracle Database Administration Contents

[Introduction to Oracle Database](https://oracle-dba-online.com/introduction_to_oracle.htm)

[Overview of Oracle Grid Architecture](https://oracle-dba-online.com/introduction_to_oracle.htm#1)
[Difference between a cluster and a grid](https://oracle-dba-online.com/introduction_to_oracle.htm#2)
[Responsibilities of Database Administrators](https://oracle-dba-online.com/introduction_to_oracle.htm#3)

**Creating Oracle Database**

[Creating Oracle 11g / 10g Database using SQL commands](https://oracle-dba-online.com/creating_the_database.htm)
[Creating Oracle Container Database in 12c using DBCA](https://oracle-dba-online.com/creating_12c_Container_DB_using_dbca.htm)
[Creating Oracle Container Database in 12c using SQL commands](https://oracle-dba-online.com/creating_12c_Container_DB_using_sql.htm)

**Managing Oracle 12c Container Databases**

[Creating User Accounts and Connecting to Oracle 12c Container and Pluggable Databases](https://oracle-dba-online.com/creating_users_in_12c.htm)

**Managing Pluggable Databases in Oracle 12c**

[Creating Pluggable Database from Seed](https://oracle-dba-online.com/Managing_Pluggable_Databases.htm#Creating Pluggable Database from Seed.)
[Cloning an Existing Pluggable Database](https://oracle-dba-online.com/Managing_Pluggable_Databases.htm#Cloning an Existing Pluggable Database)
[Unplugging and Plugging databases from one CDB to another CDB](https://oracle-dba-online.com/Managing_Pluggable_Databases.htm#Unplugging and Plugging a database from one CDB to another CDB)

[**Managing Tablespaces And Datafiles**](https://oracle-dba-online.com/tablespaces_and_datafiles.htm)

[Creating New Tablespaces](https://oracle-dba-online.com/tablespaces_and_datafiles.htm#Creating__New_Tablespaces_)
[Bigfile Tablespaces (Introduced in Oracle Ver. 10g)](https://oracle-dba-online.com/tablespaces_and_datafiles.htm#Bigfile_Tablespaces_(Introduced_in_Oracle_Ver._10g)_)
[To Extend the Size of a tablespace](https://oracle-dba-online.com/managing_tablespace_size.htm)
[To decrease the size of a tablespace](https://oracle-dba-online.com/managing_tablespace_size.htm#To_decrease_the_size_of_a_tablespace_)
[Coalescing Tablespaces](https://oracle-dba-online.com/managing_tablespace_size.htm#Coalescing_Tablespaces_)
[Taking tablespaces Offline or Online](https://oracle-dba-online.com/taking-tablespace-offline-and-online.htm)
[Making a Tablespace Read only.](https://oracle-dba-online.com/taking-tablespace-offline-and-online.htm#Making_a_Tablespace_Read_only._)
[Renaming Tablespaces](https://oracle-dba-online.com/taking-tablespace-offline-and-online.htm#Renaming_Tablespaces_)
[Dropping Tablespaces](https://oracle-dba-online.com/taking-tablespace-offline-and-online.htm#Dropping_Tablespaces_)
[Viewing Information about Tablespaces and Datafiles](https://oracle-dba-online.com/renaming-or-relocating-datafiles.htm#Viewing_Information_about_Tablespaces_and_Datafiles_)
[Relocating or Renaming Datafiles](https://oracle-dba-online.com/renaming-or-relocating-datafiles.htm#Renaming_or_Relocating_Datafiles_belonging_to_a_Single_Tablespace_)
[Renaming or Relocating Datafiles belonging to a Single Tablespace](https://oracle-dba-online.com/tablespaces_and_datafiles.htm#Renaming_or_Relocating_Datafiles_belonging_to_a_Single_Tablespace_)
[Procedure for Renaming and Relocating Datafiles in Multiple Tablespaces](https://oracle-dba-online.com/renaming-or-relocating-datafiles.htm#Procedure_for_Renaming_and_Relocating_Datafiles_in_Multiple_Tablespaces_)

**[Temporary Tablespace](https://oracle-dba-online.com/temporary-tablespace.htm)**

[Increasing or Decreasing the size of a Temporary Tablespace](https://oracle-dba-online.com/temporary-tablespace.htm#Increasing_or_Decreasing_the_size_of_a_Temporary_Tablespace_)
[Tablespace Groups](https://oracle-dba-online.com/temporary-tablespace.htm#Tablespace_Groups_)
[Creating a Temporary Tablespace Group](https://oracle-dba-online.com/temporary-tablespace.htm#Creating_a_Temporary_Tablespace_Group_)
[Assigning a Tablespace Group as the Default Temporary Tablespace](https://oracle-dba-online.com/temporary-tablespace.htm#Assigning_a_Tablespace_Group_as_the_Default_Temporary_Tablespace_)

[**Diagnosing And Repairing Locally Managed Tablespace Problems**](https://oracle-dba-online.com/repair-tablespaces.htm)

[Scenario 1: Fixing Bitmap When Allocated Blocks are Marked Free (No Overlap)](https://oracle-dba-online.com/repair-tablespaces.htm#Scenario_1:_Fixing_Bitmap_When_Allocated_Blocks_are_Marked_Free_(No_Overlap)_)
[Scenario 2: Dropping a Corrupted Segment](https://oracle-dba-online.com/repair-tablespaces.htm#Scenario_2:_Dropping_a_Corrupted_Segment_)
[Scenario 3: Fixing Bitmap Where Overlap is Reported](https://oracle-dba-online.com/repair-tablespaces.htm#Scenario_3:_Fixing_Bitmap_Where_Overlap_is_Reported_)
[Scenario 4: Correcting Media Corruption of Bitmap Blocks](https://oracle-dba-online.com/repair-tablespaces.htm#Scenario_4:_Correcting_Media_Corruption_of_Bitmap_Blocks_)
[Scenario 5: Migrating from a Dictionary-Managed to a Locally Managed Tablespace](https://oracle-dba-online.com/repair-tablespaces.htm#Scenario_5:_Migrating_from_a_Dictionary-Managed_to_a_Locally_Managed_Tablespace_)

[**Transporting Tablespaces**](https://oracle-dba-online.com/transporting-tablespaces-in-oracle.htm)

[Procedure for transporting tablespaces](https://oracle-dba-online.com/transporting-tablespaces-in-oracle.htm#Procedure_for_transporting_tablespaces_)
[Transporting Tablespace Example](https://oracle-dba-online.com/transporting-tablespaces-in-oracle.htm#Transporting_Tablespace_Example_)

[**Managing REDO LOGFILES**](https://oracle-dba-online.com/managing_redo_logfiles.htm)

[Adding a New Redo Logfile Group](https://oracle-dba-online.com/managing_redo_logfiles.htm#Adding_a_New_Redo_Logfile_Group_)
[Adding Members to an existing group](https://oracle-dba-online.com/managing_redo_logfiles.htm#Adding_Members_to_an_existing_group_)
[Dropping Members from a group](https://oracle-dba-online.com/managing_redo_logfiles.htm#Dropping_Members_from_a_group_)
[Dropping Logfile Group](https://oracle-dba-online.com/managing_redo_logfiles.htm#Dropping_Logfile_Group_)
[Resizing Logfiles](https://oracle-dba-online.com/managing_redo_logfiles.htm#Resizing_Logfiles_)
[Renaming or Relocating Logfiles](https://oracle-dba-online.com/managing_redo_logfiles.htm#Renaming_or_Relocating_Logfiles_)
[Clearing REDO LOGFILES](https://oracle-dba-online.com/managing_redo_logfiles.htm#Clearing_REDO_LOGFILES_)
[Viewing Information About Logfiles](https://oracle-dba-online.com/managing_redo_logfiles.htm#Viewing_Information_About_Logfiles_)





[**Managing Control Files**](https://oracle-dba-online.com/managing_control_files.htm)

[Multiplexing Control File](https://oracle-dba-online.com/managing_control_files.htm#Multiplexing_Control_File_)
[Changing the Name of a Database](https://oracle-dba-online.com/managing_control_files.htm#Changing_the_Name_of_a_Database_)
[Creating A New Control File](https://oracle-dba-online.com/managing_control_files.htm#Creating_A_New_Control_File)

[Cloning an Oracle Database](https://oracle-dba-online.com/cloning-oracle-database.htm)

[**Managing The UNDO TABLESPACE**](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)

[Switching to Automatic Management of Undo Space](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)
[Calculating the Space Requirements For Undo Retention](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)
[Altering UNDO Tablespace](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)
[Dropping an Undo Tablespace](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)
[Switching Undo Tablespaces](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)
[Viewing Information about Undo Tablespace](https://oracle-dba-online.com/managing_the_undo_tablespace.htm)

[**SQL Loader**](https://oracle-dba-online.com/sql_loader.htm)

[Example: Convert Data from MS-ACCESS to Oracle](https://oracle-dba-online.com/sql_loader.htm)
[Example: Migrate Data from Fixed Length file into Oracle](https://oracle-dba-online.com/sql-loader-case-studies.htm)
[Example: Convert from MySQL to Oracle](https://oracle-dba-online.com/convert-mysql-to-oracle.htm)
[Example: Migrating data from MS SQL Server to Oracle](https://oracle-dba-online.com/migrate-mssql-table-to-oracle-using-sqlloader.htm)
[Loading Data into Multiple Tables using WHEN condition](https://oracle-dba-online.com/sql-loader-case-studies.htm#_Toc176874846)
[Conventional Path Load and Direct Path Load](https://oracle-dba-online.com/sql_loader.htm)
[Direct Path](https://oracle-dba-online.com/sql_loader.htm)
[Restrictions on Using Direct Path Loads](https://oracle-dba-online.com/sql_loader.htm)

[**Export And Import**](https://oracle-dba-online.com/export_and_import.htm)

[Invoking Export and Import](https://oracle-dba-online.com/export_and_import.htm#Invoking_Export_and_Import)
[Command Line Parameters of Export tool](https://oracle-dba-online.com/export_and_import.htm#_Command_Line_Parameters_of_Export_tool)
[Example of Exporting Full Database](https://oracle-dba-online.com/export_and_import.htm#Example_of_Exporting_Full_Database)
[Example of Exporting Schemas](https://oracle-dba-online.com/export_and_import.htm#Example_of_Exporting_Schemas_)
[Exporting Individual Tables](https://oracle-dba-online.com/export_and_import.htm#Exporting_Individual_Tables)
[Exporting Consistent Image of the tables](https://oracle-dba-online.com/export_and_import.htm#Exporting_Consistent_Image_of_the_tables)

[**Using Import Utility**](https://oracle-dba-online.com/how-to-use-import-tool.htm#Using_Import_Utility)

[Example Importing Individual Tables](https://oracle-dba-online.com/how-to-use-import-tool.htm#Example_Importing_Individual_Tables)
[Example, Importing Tables of One User account into another User account](https://oracle-dba-online.com/how-to-use-import-tool.htm#Example,_Importing_Tables_of_One_User_account_into_another_User_account)
[Example Importing Tables Using Pattern Matching](https://oracle-dba-online.com/how-to-use-import-tool.htm#Example_Importing_Tables_Using_Pattern_Matching)

[Migrating a Database across platforms](https://oracle-dba-online.com/how-to-use-import-tool.htm#Migrating_a_Database_across_platforms._)

[**DATA PUMP Utility**](https://oracle-dba-online.com/data_pump_utility.htm)

[Using Data Pump Export Utility](https://oracle-dba-online.com/data_pump_utility.htm#Using_Data_Pump_Export_Utility)
[Example of Exporting a Full Database](https://oracle-dba-online.com/data_pump_utility.htm#Example_of_Exporting_a_Full_Database)
[Example of Exporting a Schema](https://oracle-dba-online.com/data_pump_utility.htm#Example_of_Exporting_a_Schema)
[Exporting Individual Tables using Data Pump Export](https://oracle-dba-online.com/data_pump_utility.htm#Exporting_Individual_Tables_using_Data_Pump_Export)
[Excluding and Including Objects during Export](https://oracle-dba-online.com/data_pump_utility.htm#Excluding_and_Including_Objects_during_Export)
[Using Query to Filter Rows during Export](https://oracle-dba-online.com/data_pump_utility.htm#Using_Query_to_Filter_Rows_during_Export)
[Suspending and Resuming Export Jobs (Attaching and Re-Attaching to the Jobs)](https://oracle-dba-online.com/data_pump_utility.htm#Suspending_and_Resuming_Export_Jobs_(Attaching_and_Re-Attaching_to_the_Jobs))



[**Data Pump Import Utility**](https://oracle-dba-online.com/data_pump_import_utility.htm)

[Importing Full Dump File](https://oracle-dba-online.com/data_pump_import_utility.htm#Importing_Full_Dump_File)
[Importing Objects of One Schema to another Schema](https://oracle-dba-online.com/data_pump_import_utility.htm#Importing_Objects_of_One_Schema_to_another_Schema)
[Loading Objects of one Tablespace to another Tablespace](https://oracle-dba-online.com/data_pump_import_utility.htm#Loading_Objects_of_one_Tablespace_to_another_Tablespace.)

[Generating SQL File containing DDL commands using Data Pump Import](https://oracle-dba-online.com/data_pump_import_utility.htm#Generating_SQL_File_containing_DDL_commands_using_Data_Pump_Import_)

[Importing objects of only a Particular Schema](https://oracle-dba-online.com/data_pump_import_utility.htm#Importing_objects_of_only_a_Particular_Schema)

[Importing Only Particular Tables](https://oracle-dba-online.com/data_pump_import_utility.htm#Importing_Only_Particular_Tables)
[Running Import Utility in Interactive Mode](https://oracle-dba-online.com/data_pump_import_utility.htm#Running_Data_Pump_Import_Utility_in_Interactive_Mode_)

[**Flash Back Features**](https://oracle-dba-online.com/flash_back_features.htm)

[Flashback Query](https://oracle-dba-online.com/flash_back_features.htm#Flashback_Query)
[Using Flashback Version Query](https://oracle-dba-online.com/flash_back_features.htm#Using_Flashback_Version_Query)
[Using Flashback Table to return Table to Past States](https://oracle-dba-online.com/flash_back_features.htm#Using_Flashback_Table_to_return_Table_to_Past_States.)
[Purging Objects from Recycle Bin](https://oracle-dba-online.com/flash_back_features.htm#Purging_Objects_from_Recycle_Bin)
[Flashback Drop of Multiple Objects With the Same Original Name](https://oracle-dba-online.com/flash_back_features.htm#Flashback_Drop_of_Multiple_Objects_With_the_Same_Original_Name)

[Flashback Database: Alternative to Point-In-Time Recovery](https://oracle-dba-online.com/oracle-flashback-database.htm)
[Enabling Flash Back Database](https://oracle-dba-online.com/oracle-flashback-database.htm#Enabling_Flash_Back_Database)
[To how much size we should set the flash recovery area](https://oracle-dba-online.com/oracle-flashback-database.htm)

[How far you can flashback database](https://oracle-dba-online.com/oracle-flashback-database.htm)

[Example: Flashing Back Database to a point in time](https://oracle-dba-online.com/oracle-flashback-database.htm)

[**Flashback Data Archive (Oracle Total Recall)**](https://oracle-dba-online.com/flashback_data_archive.htm)

I[ntroduction](https://oracle-dba-online.com/flashback_data_archive.htm)
[Creating Flashback Data Archive tablespace](https://oracle-dba-online.com/flashback_data_archive.htm#Step_1:-_Create_Flashback_data_archive_tablespace.)
[Creating Flashback Data Archive](https://oracle-dba-online.com/flashback_data_archive.htm#Step_2:-_Create_Flashback_Archive)
[Querying historical data ](https://oracle-dba-online.com/flashback_data_archive.htm#To_View_the_state_of_table_emp_10_minutes_before)

[**Log Miner**](https://oracle-dba-online.com/log_miner.htm)

[LogMiner Configuration](https://oracle-dba-online.com/log_miner.htm#LogMiner_Configuration)
[LogMiner Dictionary Options](https://oracle-dba-online.com/log_miner.htm#LogMiner_Dictionary_Options)
[Using the Online Catalog](https://oracle-dba-online.com/log_miner.htm#Using_the_Online_Catalog)
[Extracting a LogMiner Dictionary to the Redo Log Files](https://oracle-dba-online.com/log_miner.htm#Extracting_a_LogMiner_Dictionary_to_the_Redo_Log_Files)
[Extracting the LogMiner Dictionary to a Flat File](https://oracle-dba-online.com/log_miner.htm#Extracting_the_LogMiner_Dictionary_to_a_Flat_File)

[Redo Log File Options](https://oracle-dba-online.com/log_miner.htm#Redo_Log_File_Options)

[Example: Finding All Modifications in the Current Redo Log File](https://oracle-dba-online.com/log_miner.htm#Example:_Finding_All_Modifications_in_the_Current_Redo_Log_File)
[Example : Mining Redo Log Files in a Given Time Range](https://oracle-dba-online.com/log_miner.htm#Example_:_Mining_Redo_Log_Files_in_a_Given_Time_Range)

 

[**BACKUP AND RECOVERY**](https://oracle-dba-online.com/backup_and_recovery.htm)

[Opening the Database in Archivelog Mode](https://oracle-dba-online.com/backup_and_recovery.htm#Opening_or_Bringing_the_database_in_Archivelog_mode.)
[Bringing the Database again in NoArchiveLog mode](https://oracle-dba-online.com/backup_and_recovery.htm)
[Taking Offline (COLD) Backups](https://oracle-dba-online.com/backup_and_recovery.htm#TAKING_OFFLINE_BACKUPS)
[Taking Online (HOT) Backups](https://oracle-dba-online.com/backup_and_recovery.htm#TAKING_ONLINE_(HOT)_BACKUPS)
[Recovering from the Loss of a Datafile](https://oracle-dba-online.com/backup_and_recovery.htm#Recovering_from_the_lost_of_Damaged_Datafile.)
    [When the Database is running in Noarchivelog Mode](https://oracle-dba-online.com/backup_and_recovery.htm#_RECOVERING_THE_DATABASE_IF_IT_IS_RUNNING_IN_NOARCHIVELOG_MODE.)
    [When the Database is running in Archivelog Mode](https://oracle-dba-online.com/backup_and_recovery.htm#Recovering_Database_when_the_database_is_running_in_ARCHIVELOG_Mode.)

[Recovering from loss of Control File](https://oracle-dba-online.com/backup_and_recovery.htm#RECOVERING_FROM_LOST_OF_CONTROL_FILE.)

**Recovery Manager ( RMAN )**

[Taking Offline Backups using RMAN](https://oracle-dba-online.com/offline-backup-using-rman.htm)
[Recovering the Database running in NOARCHIVELOG mode using RMAN](https://oracle-dba-online.com/recovering-in-noarchivelog-rman.htm)

[Taking Online Backups using RMAN](https://oracle-dba-online.com/rman-online-backup.htm)
[Taking backups of particular tablespaces or datafiles using RMAN](https://oracle-dba-online.com/rman-partial-backup.htm)

[How to take Image Backups in RMAN](https://oracle-dba-online.com/taking-rman-image-backups.htm)

[Taking Incremental Backup using RMAN](https://oracle-dba-online.com/incremental-backup-using-oracle-rman.htm)
[Incrementally updating backup copy for fast recovery](https://oracle-dba-online.com/incremental_update_backup_copy.htm)

[View information about RMAN backups](https://oracle-dba-online.com/list-information-about-rman-backups.htm)

[Configuring Retention policy in Oracle RMAN](https://oracle-dba-online.com/retention-policy-in-oracle-rman.htm)
[Configure various Options in RMAN](https://oracle-dba-online.com/configure-settings-in-rman.htm)
[Maintaining RMAN Repository](https://oracle-dba-online.com/maintaining-rman-repository.htm)

[Recovering from the loss of datafiles using RMAN (Archivelog mode)](https://oracle-dba-online.com/recovering-datafile-archivelog-rman.htm)
[Recovering from the loss of datafile by changing it's location](https://oracle-dba-online.com/recover-datafile-by-renaming.htm)

[Performing Disaster Recovery using RMAN](https://oracle-dba-online.com/disaster-recovery-using-rman.htm)

