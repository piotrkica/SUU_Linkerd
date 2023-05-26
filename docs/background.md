Modern applications are often broken down as a network of services each
performing a specific business function. In order to execute its function, one
service might need to request data from several other services. But what if
some services get overloaded with requests? This is where a service mesh comes
in -- it routes requests from one service to the next, optimizing how all the
moving parts work together.

Service-to-service communication is what makes microservices possible. The
logic governing communication can be coded into each service without a service
mesh layer -- but as communication gets more complex, a service mesh becomes
more valuable. For cloud-native apps built in a microservices architecture, a
service mesh is a way to comprise a large number of discrete services into a
functional application.

A service mesh doesn't introduce new functionality to an app's runtime
environment -- apps in any architecture have always needed rules to specify how
requests get from point A to point B. What's different about a service mesh is
that it takes the logic governing service-to-service communication out of
individual services and abstracts it to a layer of infrastructure.

In a service mesh, requests are routed between microservices through proxies in
their own infrastructure layer. For this reason, individual proxies that make
up a service mesh are sometimes called *sidecars* since they run alongside
each service, rather than within them. Taken together, these sidecar
proxies -- decoupled from each service -- form a mesh network. A sidecar proxy
sits alongside a microservice and routes requests to other proxies. Together,
these sidecars form a mesh network.

Without a service mesh, each microservice needs to be coded with logic to
govern service-to-service communication, which means developers are less
focused on business goals. It also means communication failures are harder to
diagnose because the logic that governs interservice communication is hidden
within each service.

Every new service added to an app, or new instance of an existing service
running in a container, complicates the communication environment and
introduces new points of possible failure. Within a complex microservices
architecture, it can become nearly impossible to locate where problems have
occurred without a service mesh.

That's because a service mesh also captures every aspect of service-to-service
communication as performance metrics. Over time, data made visible by the
service mesh can be applied to the rules for interservice communication,
resulting in more efficient and reliable service requests.

For example, if a given service fails, a service mesh can collect data on how
long it took before a retry succeeded. As data on failure times for a given
service aggregates, rules can be written to determine the optimal wait time
before retrying that service, ensuring that the system does not become
overburdened by unnecessary retries.
