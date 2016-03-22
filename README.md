# security
Learning Hadoop Security

Kerberos: Kerberos uses two Servers: an authentication server that authenticates users to log in; and a ticket-granting server that provides tickets, allowing access to various resources (e.g., files or secure processes). Kerberos is incorporated into the Windows Active Directory server as its authentication mechanism.

Authentication: a login/password pair
Authorization: user may have either read, modify, or execute permissions
Encryption:

Hadoop doesn’t enable you to provide role-based access or object-level access, or offer enough granularity for attribute-level access (for a particular object
The HTTP communication between web consoles and Hadoop daemons (NameNode, Secondary NameNode, DataNode, etc.) is unencrypted and unsecured (it allows access without any form of authentication by default)
To summarize, the following threats exist for HDFS due to its architecture:
• An unauthorized client may access an HDFS file or cluster metadata via the RPC or HTTP protocols (since the communication is unencrypted and unsecured by default).
• An unauthorized client may read/write a data block of a file at a DataNode via the pipeline streaming data-transfer protocol (again, unencrypted communication).
• A task or node may masquerade as a Hadoop service component (such as DataNode) and modify the metadata or perform destructive activities.
• A malicious user with network access could intercept unencrypted internode communications.
• Data on failed disks in a large Hadoop cluster can leak private information if not handled properly.

When Hadoop daemons (or services) communicate with each other, they don’t verify that the other service is really what it claims to be.

Inherent Security Issues with Hadoop’s Job Framework
The security issues with the MapReduce framework revolve around the lack of authentication within Hadoop, the communication between Hadoop daemons being unsecured, and the fact that Hadoop daemons do not authenticate each other. The main security concerns are as follows:
• An unauthorized user may submit a job to a queue or delete or change priority of the job (since Hadoop doesn’t authenticate or authorize and it’s easy to impersonate a user).
• An unauthorized client may access the intermediate data of a Map job via its TaskTracker’s HTTP shuffle protocol (which is unencrypted and unsecured).
• An executing task may use the host operating system interfaces to access other tasks and local data, which includes intermediate Map output or the local storage of the DataNode that runs on the same physical node (data at rest is unencrypted).
• A task or node may masquerade as a Hadoop service component such as a DataNode, NameNode, JobTracker, TaskTracker, etc. (no host process authentication).
• A user may submit a workflow (using a workflow package like Oozie) as another user (it’s easy to impersonate a user).

Unencrypted Data in Transit
No Data Encryption at Rest
Hadoop Doesn’t Track Data Provenance: Data provenance is a process that captures how data is processed through the workflow and aids debugging by enabling backward tracing—finding the input data that resulted in output for any given step. If the output is unusual (or not what was expected), backward tracing can be used to determine the input that was processed.

Using Hadoop Logging for Security:
  • HDFS audit logs (to record all HDFS access activity within Hadoop),
  • MapReduce audit logs (record all submitted job activity), and
  • Hadoop daemon log files for NameNode, DataNode, JobTracker and TaskTracker.

The Log4j API is at the heart of Hadoop logging, be it audit logs or Hadoop daemon logs.

You can easily change the logging level for a Hadoop daemon at its URL. For example, http://jobtracker-host:50030/logLevel will change the logging level while this daemon is running, but it will be reset when it is restarted.

Hadoop metrics that you can use for security purposes:
• Activity statistics on the NameNode
• Activity statistics for a DataNode
• Detailed RPC information for a service
• Health monitoring for sudden change in system resources

Ganglia focuses on gathering metrics and tracking them over a time period, while Nagios focuses more on being an alerting mechanism. Because gathering metrics and alerting both are essential aspects of monitoring, they work best in conjunction. Both Ganglia and Nagios have agents running on all hosts for a cluster and gather information.
