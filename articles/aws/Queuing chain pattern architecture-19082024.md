# Queuing chain pattern architecture

The queuing chain pattern is ideal for scenarios where tasks need to be processed sequentially across multiple systems. Take an image-processing application as an example. The process involves capturing an image, watermarking it, generating different resolutions, and creating thumbnails. These steps are closely linked, so a failure in one can disrupt the entire operation.

![Queuing chain pattern architecture](/architecture-diagrams/aws/queuing-chain-pattern-architecture.png)

By using queues between each step, you can avoid a single point of failure and design a more resilient system. This pattern also allows multiple servers to process tasks in parallel, increasing efficiency. For instance, AWS's Amazon Simple Queue Service (SQS) can be used to manage this workflow:

1. When an image is uploaded, a fleet of Amazon EC2 servers watermarks it and pushes the processed image into an Amazon SQS queue.
2. Another fleet of EC2 servers pulls the watermarked images from the queue.
3. These servers then process the image to create multiple resolution versions.
4. Once done, the servers push the message into another Amazon SQS queue.
5. After processing, the message is deleted from the previous queue to free up space.
6. Finally, a third fleet of EC2 servers retrieves the encoded images from the queue to create thumbnails and add copyrights.

This architecture offers several benefits:

- **Loosely Coupled Processing:** By using asynchronous processing, the system can return responses quickly without needing to wait for other services.
- **Reliability:** Even if an EC2 instance fails, the message stays in the queue, allowing the process to resume upon recovery, ensuring robustness against failures.
- **Scalability:** The system can automatically adjust to fluctuations in demand by scaling workloads based on queue message loads.

Overall, the queuing chain pattern makes your system more reliable, scalable, and efficient by handling tasks in a decoupled and resilient manner.
