# Eureka Service Discovery Server: Scenario-Based & Troubleshooting Interview Questions

---

## 1. Service Registration Issues
**Q:**  
A new microservice is not appearing in the Eureka dashboard after deployment. What steps would you take to troubleshoot this issue?

**A:**  
- Check if the microservice has the correct Eureka client dependency.
- Verify the Eureka server URL in the client's configuration.
- Ensure the client's `application.properties` has `eureka.client.register-with-eureka=true`.
- Check network connectivity between the client and Eureka server.
- Review logs for registration errors on both client and server.

---

## 2. Eureka Server High Availability
**Q:**  
How would you set up Eureka for high availability in a production environment? What are the key configuration changes?

**A:**  
- Deploy multiple Eureka server instances.
- Configure each server to register with the others (`eureka.client.service-url.defaultZone`).
- Use a load balancer in front of the Eureka servers.
- Ensure all clients point to the load balancer or all Eureka instances.

---

## 3. Stale Service Instances
**Q:**  
You notice that some services listed in Eureka are no longer running, but still appear as "UP" in the dashboard. Why does this happen and how can you fix it?

**A:**  
- The Eureka server hasn't received a heartbeat from the service but hasn't timed it out yet.
- Adjust the `eureka.instance.lease-expiration-duration-in-seconds` and `eureka.instance.lease-renewal-interval-in-seconds` properties.
- Ensure services shut down gracefully and deregister from Eureka.

---

## 4. Eureka Server Down
**Q:**  
What happens to service discovery if the Eureka server goes down? How do you ensure resilience?

**A:**  
- Clients use their local cache for service discovery (read-only mode).
- No new registrations or deregistrations are possible.
- To ensure resilience, run multiple Eureka servers and configure clients to connect to all.

---

## 5. Network Partition Scenario
**Q:**  
If there is a network partition between some microservices and the Eureka server, what issues might arise? How does Eureka handle this?

**A:**  
- Services unable to reach Eureka will not renew their lease and may be marked as "DOWN" after expiration.
- Clients may use stale registry data.
- Eureka's self-preservation mode can prevent mass expiration during network issues.

---

## 6. Self-Preservation Mode
**Q:**  
What is Eureka's self-preservation mode? When does it activate, and what are its pros and cons?

**A:**  
- Activates when a large percentage of clients stop sending heartbeats.
- Prevents mass removal of instances from the registry.
- Pros: Avoids accidental mass deregistration during network issues.
- Cons: May show dead instances as "UP".

---

## 7. Version Compatibility
**Q:**  
A client microservice is using an older version of the Eureka client library and is unable to register with the server. What could be the cause?

**A:**  
- Incompatibility between Eureka server and client versions.
- Check release notes for breaking changes.
- Upgrade the client or adjust server compatibility settings.

---

## 8. Custom Metadata
**Q:**  
How can you add custom metadata (e.g., version, region) to a service registration in Eureka? How can other services use this metadata?

**A:**  
- Add metadata in the client's `application.properties` using `eureka.instance.metadata-map`.
- Other services can query Eureka for instances with specific metadata.

---

## 9. Security Concerns
**Q:**  
How would you secure your Eureka server so that only authorized services can register?

**A:**  
- Use Spring Security to protect the Eureka endpoints.
- Implement authentication (e.g., HTTP Basic Auth, OAuth2).
- Restrict network access to the Eureka server.

---

## 10. Service Deregistration
**Q:**  
A service is shutting down for maintenance. How can you ensure it is properly deregistered from Eureka?

**A:**  
- Implement graceful shutdown logic in the service.
- Use the `DiscoveryClient.shutdown()` method or ensure the Spring context closes cleanly.

--- 