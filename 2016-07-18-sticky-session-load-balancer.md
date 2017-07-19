### Sticky Session and Load Balancer

Sticky session: what’s the issues 
1. scalability - Pros and Cons of Sticky Session / Session Affinity load blancing strategy? - Stack Overflow( http://stackoverflow.com/questions/1553645/pros-and-cons-of-sticky-session-session-affinity-load-blancing-strategy
2. sticky session vs. db performance issue? 
    1. session is loaded once into memory/cache and read multiple times
    2. memcache session: 针对sticky session，中心session，但是用memcached做中心（cached）
3. 如何sessionless
4. spike? use ip? how to address
5. 如何用dns实现load balancing？dns round robin

----------------------------
In high scalable website design, load balancer is a critical component to the website's scalibility. A common load balancer is designed to dispatch a request to one of hundreds of backend servers. A challenge that this approach presents is a server is usually state-aware by saving requrest states of a user in a session. However, if a load balancer can't know the session, it might not dispatch a second request of the same user to the same server.

"Sticky session", aka "session affinity" is one way to solve the problem by bringing states to load balancer. Load balancer needs to locate the server that has the user session. This usually violates the load balancer design principles:

1. stickiness makes the traffic splitting based on user session rather than the number of requests, hence, some servers can get unequal load
2. a new web server online may not take much load as most requests are distributed to devoted servers

Ideally, we want to have user session shared between web servers so that load balancer can redirect a request to any server without considering the user session associated to the request. 

A common way of achieving this is do not store session data in web servers but in a separate service: memcache or database or both depending on the performance/persistent requirement. By doing that, load balancer can send a request to any web server and the web server only retrieves the session from db or memcache. An added benefit is a down web server is no longer a worry as state is moving out to a central service.
