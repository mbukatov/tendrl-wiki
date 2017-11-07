Response Times and Latency can adversely affect negatively affect an user’s experience, so it’s important to try to achieve as optimal a performance as possible.  Poor response time or high latency can result in
* drop in product use
* poor product perception

The following are suggested best practices for acceptable response times and latency that all combine to deliver acceptable if not good user experience.

* UI response times should be as fast as possible
    * Application launch, i.e. time when the user initiates to start the application, in < 10 sec
    * Response to user action in <= 1 sec
        * 0.2 sec gives the felling of instantaneous response
        * 1.0 second is about the limit for user's flow of thought to stay uninterrupted, even though the user will notice the delay. Normally, no special feedback is necessary for delays < 1 sec, but the user does lose the feeling of operating directly with the System.
    * Generating a report, e.g. requesting a batch report job to be run, in < 30 seconds
    * For any responses > 5 sec, the System should provide feedback indicating when it expects to be done, otherwise users will not know what to expect.
        * 10 sec is about the limit for keeping the user's attention focused. For longer delays, users will want to perform other tasks while waiting for the System to finish, so they should be given feedback indicating when the computer expects to be done. 
        * Feedback during the delay is especially important if the response time is likely to be highly variable, since users will then not know what to expect.
* Alerts latency
    * Alerts should be visible in < 10 sec and not exceed 60 sec.
    * As a rule of thumb, the average alert latency should be < 60 sec in a well-performing system, but event latency of between 60 to 90 sec is also acceptable.
* Network latency
    * Up to 150 ms: great user experience
    * 150ms – 300 ms: good/acceptable user experience
    * Over 300 ms: degraded user experience
    * Minimize network latency between the infrastructure components to < 8 ms and does not exceed 200 ms.  The minimum bandwidth is ? Kbps with n% utilization for the agents and etcd central store.
    * Ensure latency between UI and infrastructure < 50 ms
    * Higher latency will result in slower but error-free performance and can potentially cause slow updates to the etcd central store in large deployments or stretch configurations.
   


**References**
* [Acceptable waiting time for users in time sensitive actions](https://ux.stackexchange.com/questions/58163/acceptable-waiting-time-for-users-in-time-sensitive-actions)
* [Analyzing SCOM 2012 Alert logging latency](https://blogs.technet.microsoft.com/dirkbri/2014/08/19/analyzing-scom-2012-alert-logging-latency/)
* [Essential Microsoft Operations Manager](https://books.google.com/books?id=7v1dQCX4HEIC&pg=PA296&lpg=PA296&dq=system+logging+acceptable+latency&source=bl&ots=8yAhts8K0q&sig=NfssIsOywt79PhcW_1RqqXojk4k&hl=en&sa=X&ved=0ahUKEwiFnNixka3XAhVilVQKHb_rDqUQ6AEIZTAJ#v=onepage&q=system%20logging%20acceptable%20latency&f=false)
* [How Network Latency Impacts User Experience](https://www.citrix.com/blogs/2017/09/25/how-network-latency-impacts-user-experience/)
* [Website Response Times](https://www.nngroup.com/articles/website-response-times/)
* [What is the expected response time to deliver good user experience?
](https://www.dynatrace.com/blog/what-is-the-expected-response-time-for-a-good-user-experience/)
