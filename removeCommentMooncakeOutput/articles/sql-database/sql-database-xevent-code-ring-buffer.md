<properties 
	pageTitle="XEvent Ring Buffer code for SQL Database | Windows Azure" 
	description="Provides a Transact-SQL code sample that is made easy and quick by use of the Ring Buffer target, in Azure SQL Database." 
	services="sql-database" 
	documentationCenter="" 
	authors="MightyPen" 
	manager="jeffreyg" 
	editor="" 
	tags=""/>


<tags 
	ms.service="sql-database" 
	ms.date="10/22/2015" 
	wacn.date=""/>


# Ring Buffer target code for extended events in SQL Database


You want a complete code sample for the easiest quick way to capture and report information for an extended event during a test. The easiest target for extended event data is the [Ring Buffer target](http://msdn.microsoft.com/zh-cn/library/ff878182.aspx).


This topic presents a Transact-SQL code sample that:


1. Creates a table with data to demonstrate with.

2. Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.
 - The event is limited to SQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.
 - Chooses to send the output of the event to a target of type Ring Buffer, namely  **package0.ring_buffer**.

3. Starts the event session.

4. Issues a couple of simple SQL UPDATE statements.

5. Issues an SQL SELECT to retrieve event output from the Ring Buffer.
 - **sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.

6. Stops the event session.

7. Drops the Ring Buffer target, to release its resources.

8. Drops the event session and the demo table.


## Prerequisites


- An Azure account and subscription. You can sign up for a [trial](/pricing/1rmb-trial).


- Any database you can create a table in.
 - Optionally you can [create an **AdventureWorksLT** demonstration database](/documentation/articles/sql-database-get-started) in minutes.


- SQL Server Management Studio (ssms.exe), its August 2015 Preview or a later version. 
You can download the latest ssms.exe from:
 - [A link in topic.](http://msdn.microsoft.com/zh-cn/library/mt238290.aspx)
 - [A direct link to the download.](http://download.microsoft.com/download/0/6/4/06471151-0668-485E-8CD0-D7D8297EE357/SSMS_BurnInstaller_June2015/SSMS-Web-Setup.exe)
 - Microsoft recommends that you update your ssms.exe periodically.


## Code sample


With very minor modification, the following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server. The difference is the presence of the node '_database' in the name of some dynamic management views (DMVs) in the FROM clause in Step 5. For example:

- sys.dm_xe**_database**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
	(SELECT * FROM sys.objects
		WHERE type = 'U' and name = 'tabEmployee')
BEGIN
	DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
	EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
	EmployeeId           int                not null  identity(1,1),
	EmployeeKudosCount   int                not null  default 0,
	EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
	VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
	(SELECT * from sys.database_event_sessions
		WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
	DROP EVENT SESSION eventsession_gm_azuresqldb51
		ON DATABASE;
END
GO


CREATE
	EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	ADD EVENT
		sqlserver.sql_statement_starting
			(
			ACTION (sqlserver.sql_text)
			WHERE statement LIKE '%UPDATE tabEmployee%'
			)
	ADD TARGET
		package0.ring_buffer
			(SET
				max_memory = 500   -- Units of KB.
			);
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
	SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
	SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
		se.name  AS [session-name],
		ev.event_name,
		ac.action_name,
		st.target_name,
		se.session_source,
		st.target_data,
		CAST(st.target_data AS XML)  AS [target_data_XML]
	FROM
				   sys.dm_xe_database_session_event_actions  AS ac
		INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name            = ac.event_name
		                                                           AND ev.event_session_address = ac.event_session_address
		INNER JOIN sys.dm_xe_database_session_object_columns AS oc  ON oc.event_session_address = ac.event_session_address
		INNER JOIN sys.dm_xe_database_session_targets        AS st  ON st.event_session_address = ac.event_session_address
		INNER JOIN sys.dm_xe_database_sessions               AS se  ON ac.event_session_address = se.address
	WHERE
		oc.column_name = 'occurrence_number'
		AND
		se.name        = 'eventsession_gm_azuresqldb51'
		AND
		ac.action_name = 'sql_text'
	ORDER BY
		se.name,
		ev.event_name,
		ac.action_name,
		st.target_name,
		se.session_source;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## Ring Buffer contents


We used ssms.exe to run the code sample.


To view the results, we clicked the cell under the column header **target_data_XML**.

Then in the results pane we clicked the cell under the column header **target_data_XML**. This created another file tab in ssms.exe in which the content of the result cell was displayed, as XML.


The output is shown in the following block. It looks long, but it is just two **<event>** elements.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### Release resources held by your Ring Buffer


When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like the following:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	DROP TARGET package0.ring_buffer;
GO
```


The definition of your event session is updated, but not dropped. Later you can add another instance of the Ring Buffer to your event session:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
	ON DATABASE
	ADD TARGET
		package0.ring_buffer
			(SET
				max_memory = 500   -- Units of KB.
			);
```


## More information


The primary topic for extended events on Azure SQL Database is:


- [Extended event considerations in SQL Database](/documentation/articles/sql-database-xevent-db-diff-from-svr), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.


Other code sample topics for extended events are available at the following links. However, you must routinely check any sample to see whether the sample targets Microsoft SQL Server versus Azure SQL Database. Then you can decide whether minor changes are needed to run the sample.


- Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](/documentation/articles/sql-database-xevent-code-event-file)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/zh-cn/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/zh-cn/library/bb630355.aspx)
-->
