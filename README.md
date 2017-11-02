# stop-malicious-http-traffic-with-Istio
It's always been a critical task to secure a your web servcies. In the world of microservices, it's been even more challenging. Many orchastration tools, like Kubernetes, lack the ability to detect and react to malicious attacks, especially at serice/pod level. Forturnetely the emerging of new technologies like Istio makes that task possible. Istio enriches the capabilities of managing service mesh. The two important features used is pattern are: Collecting metrics and enforcing policies at pod level. 

In this pattern, the malicous traffic we address is not the tranditional Ddos attacks, which happens usually at level 3 or 4.  Honestly, it's beyond the capability of either Kubernetes or Istio to deal with millions of requests per second. It is the job of the Infruasture provider to address these problems. The traffic we address here are normally smaller in scale but more damaging to the services at layer 5-7.

Below are the traffic scenarios this pattern is targeting:
a. The client is using curl to frequently access the service. This is an indiator that the client is a bot than a customer, which normally uses web browser to access the service. Of course, there are other tools like "wget", but here we are just setting an example. Our solution will be if the curl happen too frequently, the service will be denied.    
b. The client is accessing the service too frequently. This scenario is like a simple Ddos attack, where the client will access the services way more than human can process the information.  In this case, we also set a threshold. If the client goes over that limit, the IP address will be banned from accessing the service.   
c. Hacking: The client will try to "guess" the common URI so it can either do sql injection or getting information out of the service. During the prep of this pattern, my cluster was actually attacked. Of course they got nothing but we all know now how real it is. The common symptom is they would try to access url/db or url/sql, and then get a 40x error from the server. The solution is to put a low threshold for 40x errors for each ip and block the ip if it goes over the threshold.

