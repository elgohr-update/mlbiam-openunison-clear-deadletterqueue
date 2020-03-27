# Clearing OpenUnison's Dead Letter Queue

OpenUnison processes all tasks in workflows as messages on a queue.  This makes OpenUnison both scalable and reliable.  If at any point a task fails, such as if OpenUnison can't talk to an api, database, or directory, then the failed messages will go into a "Dead Letter Queue" for safe keeping until the issue is resolved.  Most messaging solutions provide a way to move messages one-at-a-time, but its easier to move the messages back to their original queues in bulk.  This container, when used with the `Job` in src/main/yaml will move all the messages in your dead letter queue back into their original queues.

Prior to running the `Job`, make the following updates:

`spec.template.spec.initContainers[0].image` - This should be the same image you are using in your OpenUnison deployment
`spec.template.spec.containers[0].command[12]` - The name of the queue to clear

Once you create the `Job`, it will copy the OpenUnison configuration from your OpenUnison pod then run the DLQ utility using it.  The output will look something like:

```
[2020-03-27 15:19:01,340][main] INFO  OpenUnisonUtils - Loading Unison Configuration
[2020-03-27 15:19:01,780][main] INFO  OpenUnisonConfigLoader - No config from include files, using original
[2020-03-27 15:19:01,993][main] INFO  OpenUnisonUtils - Configuration loaded
[2020-03-27 15:19:01,993][main] INFO  OpenUnisonUtils - Loading the keystore...
[2020-03-27 15:19:02,402][main] INFO  OpenUnisonUtils - ...loaded
[2020-03-27 15:19:02,402][main] INFO  OpenUnisonUtils - Getting the DLQ Name...
[2020-03-27 15:19:02,408][main] INFO  QueUtils - DLQ Run : c9a2af7d-4ee5-4001-9779-c58c6e0e7f6b
[2020-03-27 15:19:02,408][main] INFO  QueUtils - Connecting to com.tremolosecurity.provisioning.jms.providers.AwsSqsConnectionFactory
[2020-03-27 15:19:03,040][main] INFO  QueUtils - Connected
[2020-03-27 15:19:03,040][main] INFO  QueUtils - Creating queue openunison-dlq-eks.fifo
[2020-03-27 15:19:03,522][main] INFO  QueUtils - Checking for messages
[2020-03-27 15:19:04,523][main] INFO  SQSMessageConsumer - Shutting down ConsumerPrefetch executor
```

Once the `Job` is done, it can be deleted.