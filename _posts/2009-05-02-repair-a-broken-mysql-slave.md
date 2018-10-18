---
id: 122
title: Repair a broken MySQL slave
date: 2009-05-02T14:30:08+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=122
permalink: /2009-05-02/repair-a-broken-mysql-slave/
categories:
  - Notebook
tags:
  - MySQL
  - Repair
  - Replication
  - Slave
---
If a MySQL slave encounters an error while replicating commands from the master, the slave will abort.

One way this can happen is if you are using triggers on a table that calls a stored procedure, but the stored procedures are missing on the slave because you forgot to include the &#8211;routines option when generating a mysqldump from the master to import to the slave.

So what do you do? First, verify that the slave is indeed encountering errors:

<pre>mysql&gt; SHOW SLAVE STATUS \G
*************************** 1. row ***************************
             Slave_IO_State: Waiting for master to send event
                Master_Host: 1.2.3.4
                Master_User: slave_user
                Master_Port: 3306
              Connect_Retry: 60
            Master_Log_File: mysql-bin.001079
        Read_Master_Log_Pos: 269214454
             Relay_Log_File: slave-relay.000130
              Relay_Log_Pos: 100125935
      Relay_Master_Log_File: mysql-bin.001079
           Slave_IO_Running: Yes
          Slave_SQL_Running: No
            Replicate_Do_DB: mydb
        Replicate_Ignore_DB:
         Replicate_Do_Table:
     Replicate_Ignore_Table:
    Replicate_Wild_Do_Table:
Replicate_Wild_Ignore_Table:
                 Last_Errno: 1146
                 Last_Error: Error 'Table 'mydb.taggregate_temp_1212047760' doesn't exist' on query. Default database: 'mydb'. 
Query: 'UPDATE thread AS thread,taggregate_temp_1212047760 AS aggregate
        SET thread.views = thread.views + aggregate.views
        WHERE thread.threadid = aggregate.threadid'
               Skip_Counter: 0
        Exec_Master_Log_Pos: 203015142
            Relay_Log_Space: 166325247
            Until_Condition: None
             Until_Log_File:
              Until_Log_Pos: 0
         Master_SSL_Allowed: No
         Master_SSL_CA_File:
         Master_SSL_CA_Path:
            Master_SSL_Cert:
          Master_SSL_Cipher:
             Master_SSL_Key:
      Seconds_Behind_Master: NULL
1 row in set (0.00 sec)

mysql&gt;</pre>

If  either Slave\_IO\_Running or Slave\_SQL\_Running are &#8216;No&#8217; then the slave has aborted. Here&#8217;s how to fix it:

## Stop the slave

<pre><pre>mysql&gt; STOP SLAVE;</pre>


<h2>
  Resolve the error
</h2>


<p>
  Pay attention to the Last_Error field and try to resolve it. If this is a recurring issue you may need to import a fresh dump from the master to bring your slave back into sync. If the situation merits it, you can run the following to have the slave skip the offending SQL query:
</p>


<pre>mysql&gt; SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;</pre>


<p>
  Be careful, though because this could have severe repercussions down the line.
</p>


<h2>
  Restart the slave
</h2>


<pre>mysql&gt; START SLAVE;</pre>


<h2>
  Verify that the slave is running
</h2>


<pre>mysql&gt; SHOW SLAVE STATUS \G</pre>


<p>
  When Slave_IO_Running and Slave_SQL_Running both are set to &#8216;Yes&#8217; then you&#8217;re good to go. It would probably be a good idea to have some sort of monitor to ensure your slaves are indeed running when you need them to. Perhaps I&#8217;ll throw together a quick script to do that&#8230;
</p>


<p>
  Thanks to <a href="http://www.howtoforge.com/how-to-repair-mysql-replication" target="_blank">http://www.howtoforge.com/how-to-repair-mysql-replication</a> for this information.
</p>