# Job observer pattern

![Job observer pattern](/architecture-diagrams/aws/job-observer-pattern-architecture.png)

The queuing chain pattern offers a loosely coupled architecture, but managing workload spikes requires an adaptive approach. This is where the job observer pattern comes in. In this pattern, you can create an auto-scaling group based on the number of messages in the queue, allowing your system to adjust processing power according to demand.
In a typical setup, one fleet of Amazon EC2 servers handles batch jobs and adds messages, such as image metadata, to the queue. Another fleet consumes these messages for tasks like image encoding. When the number of messages in the queue reaches a certain threshold, Amazon CloudWatch triggers auto-scaling to add more servers to the consumer fleet, speeding up processing. As the workload decreases, auto-scaling reduces the number of servers, optimizing costs.

The job observer pattern ensures that your system remains efficient and cost-effective, with the ability to complete jobs faster by scaling resources based on the job size. This approach also adds resilience to your architecture, as job processing continues even if a server fails.

While a queue-based architecture provides loose coupling and relies on the asynchronous pull method for message processing, the job observer pattern enhances it by dynamically scaling resources to match the workload. This ensures that your system maintains high performance and responsiveness, regardless of fluctuations in demand.
