# Time Travel
Time travel allows you change the view of the data to a previous time. This is not the same as an `AS OF` query commonly used in SQL. The data is rolled back to a prior time.

Let us look at the snapshots available for the customer table in the workshop schema. We currently have just 1 snapshot. First make sure you are in the proper directory.
```
cd /root/ibm-lh-dev/bin
```
Connect to Presto using the workshop schema.
```
./presto-cli --catalog iceberg_data --schema workshop
```
Check current snapshots – STARTING STATE.
```
SELECT 
   * 
FROM 
   iceberg_data.workshop."customer$snapshots" 
ORDER BY 
   committed_at;
```
<pre style="font-size: small; color: darkgreen; overflow: auto">
        committed_at         |     snapshot_id     | parent_id | operation |                                               manifest_list                                                |                                                                                                                summary                                                                                                                
-----------------------------+---------------------+-----------+-----------+------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 2023-06-05 18:30:12.994 UTC | 6243511110201494487 | NULL      | append    | s3a://iceberg-bucket/customer/metadata/snap-6243511110201494487-1-b5ab84dc-671a-426a-a734-940baa49a11f.avro | {changed-partition-count=1, added-data-files=1, total-equality-deletes=0, added-records=1500, total-position-deletes=0, added-files-size=75240, total-delete-files=0, total-files-size=75240, total-records=1500, total-data-files=1} 
(1 row)
</pre>
Capture the first snapshot ID returned by the SQL statement. You will need this value when you run the rollback command.
```
SELECT 
   snapshot_id 
FROM 
   iceberg_data.workshop."customer$snapshots" 
ORDER BY 
   committed_at;
```
<pre style="font-size: small; color: darkgreen; overflow: auto">
     snapshot_id     
---------------------
 6243511110201494487 
(1 row)
</pre>

Remember that number that was returned with the query above. Insert the following record to change the customer table in the workshop schema. 
```
insert into customer 
  values(1501,'Deepak','IBM SVL',16,'123-212-3455',
         123,'AUTOMOBILE','Testing snapshots');
```
 
Let us look at the snapshots available for the customer table in the workshop schema. You should have 2 snapshots. 
```
SELECT 
   * 
FROM 
   iceberg_data.workshop."customer$snapshots" 
ORDER BY 
   committed_at;
```
<pre style="font-size: small; color: darkgreen; overflow: auto">
        committed_at         |     snapshot_id     |      parent_id      | operation |                                               manifest_list                                                |                                                                                                                summary                                                                                                                
-----------------------------+---------------------+---------------------+-----------+------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 2023-06-05 18:30:12.994 UTC | 6243511110201494487 | NULL                | append    | s3a://iceberg-bucket/customer/metadata/snap-6243511110201494487-1-b5ab84dc-671a-426a-a734-940baa49a11f.avro | {changed-partition-count=1, added-data-files=1, total-equality-deletes=0, added-records=1500, total-position-deletes=0, added-files-size=75240, total-delete-files=0, total-files-size=75240, total-records=1500, total-data-files=1} 
 2023-06-05 18:52:49.193 UTC | 7110570704088319509 | 6243511110201494487 | append    | s3a://iceberg-bucket/customer/metadata/snap-7110570704088319509-1-ef26bcf1-c122-4ea4-86b7-ba26369be374.avro | {changed-partition-count=1, added-data-files=1, total-equality-deletes=0, added-records=1, total-position-deletes=0, added-files-size=1268, total-delete-files=0, total-files-size=76508, total-records=1501, total-data-files=2}     
(2 rows)
</pre>

Querying the customer table in the workshop schema, we can see the record inserted with name=’Deepak’.
```
select * from customer where name='Deepak';
```
<pre style="font-size: small; color: darkgreen; overflow: auto">
 custkey |  name  | address | nationkey |    phone     | acctbal | mktsegment |      comment      
---------+--------+---------+-----------+--------------+---------+------------+-------------------
    1501 | Deepak | IBM SVL |        16 | 123-212-3455 |   123.0 | AUTOMOBILE | Testing snapshots 
(1 row)
</pre>

We realize that we don’t want the recent updates or just want to see what the data was at any point in time to respond to regulatory requirements. We will leverage the out-of-box system function `rollback_to_snapshot` to rollback to an older snapshot. The syntax for this function is:
<code style="color:blue; font-size: small">CALL iceberg_data.system.rollback_to_snapshot('workshop','customer',x);</code>

The "x" would get replaced with the <code style="color:blue;font-size:medium;">snapshot_id</code> number that was found in the earlier query. It will be different on your system than the examples above.

Copy the next code segment into Presto.
```
CALL iceberg_data.system.rollback_to_snapshot('workshop','customer',
```
You will see output similar to the following:
<pre style="font-size: small; color: darkgreen; overflow: auto">
CALL iceberg_data.system.rollback_to_snapshot('workshop','customer',
              -> 
</pre>
At this point you will need to copy and paste your `snapshot_id` into the Presto command line and press return or enter. You will see following:
<pre style="font-size: small; color: darkgreen; overflow: auto">
CALL iceberg_data.system.rollback_to_snapshot('workshop','customer',
              -> 7230522396120575591
7230522396120575591
</pre>
Now you will need to terminate the command with a `);` to see the final result.
```
);
```
<pre style="font-size: small; color: darkgreen; overflow: auto">
CALL iceberg_data.system.rollback_to_snapshot('workshop','customer',
              -> 7230522396120575591
7230522396120575591
              -> );
);
CALL
</pre>

Querying the customer table in the workshop schema, we cannot see the record inserted with name=’Deepak’.
```
select * from customer where name='Deepak';
```
<pre style="font-size: small; color: darkgreen; overflow: auto">
 custkey |  name  | address | nationkey |    phone     | acctbal | mktsegment |      comment      
---------+--------+---------+-----------+--------------+---------+------------+-------------------
(0 rows)
</pre>
Quit Presto.
```
quit;
```