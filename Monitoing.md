## Monitoring

This is a summary from the SRE book monitoring section.

### Black box monitoing 
Black box monitoing refers to observing a system from the outside, without any knowledge of its internal workings. It typically involves checking the system’s external endpoints or user-facing interfaces to verify that they are functioning as expected. This approach is similar to how an end user would interact with the system, making it useful for detecting outages or issues that affect user experience.

### White box monitoing
White box monitoing involves monitoring the internal state and metrics of a system. This can include tracking resource usage, application logs, or custom metrics that provide insight into the system’s health and performance. White box monitoring allows teams to detect and diagnose issues before they impact users, as it provides visibility into the inner workings of the system.

---

### Four Golden Signals of Monitoring

The SRE book identifies four key signals to monitor for any service:

1. **Latency:** The time it takes to service a request. (e.g., API requests latency).
2. **Traffic:** The demand placed on your system (e.g., requests per second).
3. **Errors:** The rate of failed requests or incorrect responses. (e.g., HTTP status codes).
4. **Saturation:** How "full" your service is (e.g., resource utilization like CPU, memory, or disk).

Monitoring these four signals helps teams quickly identify and respond to issues, ensuring the reliability and performance of their systems.