# SAP Sales Cloud - Performance Best Practices

## Performance 101


[This blog](https://blogs.sap.com/2018/01/23/c4c-performance-101/) walks through the process of understanding and analyzing performance in SAP Hybris Cloud for Customer (C4C). It also describes the various performance metric attributes and provides link(s) on what to do next.


## Basic Analysis


[How-To: View performance statistics of the last user interaction](https://blogs.sap.com/2018/01/23/how-to-view-performance-statistics-of-the-last-user-interaction/)

[How-To: Troubleshoot Cold Browsers](https://blogs.sap.com/2018/01/23/sap-cloud-for-customer-how-to-troubleshoot-cold-browser-scenarios/)

[How-To: How to identify latency and bandwidth issues](https://blogs.sap.com/2018/01/23/how-to-identify-high-network-latency-and-bandwidth-within-sap-cloud-for-customers/)

[Information required](https://blogs.sap.com/2017/08/09/information-required-by-sap-when-submitting-incidents-related-to-performance-for-sap-hybris-cloud-for-customer/) by SAP when submitting performance incidents

## Expert Analysis     

### User Experience/ Client

How-To: [Troubleshoot High Client time](https://blogs.sap.com/2018/01/17/sap-hybris-cloud-for-customer-troubleshoot-high-client-time/)
How-To: [Troubleshoot high load time of home page](https://blogs.sap.com/2017/12/13/sap-hybris-cloud-for-customer-troubleshoot-high-load-time-of-home-page/)
How-To: [Troubleshoot high run time in reporting](https://blogs.sap.com/2018/01/22/best-practices-for-sap-cloud-for-customer-analytics-reports-performance/)
How-To: [Disable payload compression for troubleshooting purposes](https://blogs.sap.com/2018/01/23/how-to-disable-post-request-compression/)

### Customizations

How-To: [Troubleshoot PDI/ Cloud SDK](https://blogs.sap.com/2015/08/27/sap-cloud-application-studio-performance-best-practices/)
How-To: [Troubleshoot high Web Service time](https://blogs.sap.com/2018/01/19/how-to-troubleshoot-external-web-service-calls/)
How-To: [High Workflow time](https://blogs.sap.com/2017/08/24/sap-hybris-cloud-for-customer-workflow-best-practices/)
  
### Network

How-To: [Identify internal vs external networks during troubleshooting](https://blogs.sap.com/2018/01/23/how-to-identify-internal-vs-external-networks/)
How-to: [Troubleshoot network issues](https://blogs.sap.com/2018/01/23/how-to-troubleshoot-high-latency-networks-for-sap-cloud-for-customer/)
How-To: [Identify high network latency and bandwidth](https://blogs.sap.com/2018/01/23/how-to-identify-high-network-latency-and-bandwidth-within-sap-cloud-for-customers/)
[How different network conditions could influence the performance of Cloud for Customers](https://blogs.sap.com/2019/06/14/how-different-network-conditions-could-influence-the-performance-of-cloud-for-customers/)
[How Akamai works in SAP Cloud for Customers](https://blogs.sap.com/2019/06/10/how-akamai-works-in-sap-cloud-for-customers/)

### Proactive Troubleshooting

How-To: [Use Solution Diagnostics for identifying performance bottlenecks](https://blogs.sap.com/2017/12/12/sap-hybris-cloud-for-customer-solution-diagnostics/)

### API/ Integration

How-To: [Use of ODATA response header to analyze backend response time](https://blogs.sap.com/2018/01/22/how-to-get-backend-server-response-times-from-odata-calls-in-c4c/)

Performance Updates
New! Performance improvements in 1802 

1802: https://blogs.sap.com/2018/02/15/1805-performance-update/

1711: https://blogs.sap.com/2017/10/22/whats-new-in-c4c-performance-1711-edition/

1708: Performance improvements include request payload compression and faster loading of static assets with CDN.

1705: Enhancements include reduce cold starts wrt. implicit personalization, update of newer SAP UI libs, scrolling improvements in Fiori client

1702: Enhancements include improvements in server times, mobile offline, TTI, pre-fetch of UI floorplan metadata

Performance Best Practices


#1 Browser Cache Clearing


When a users browser has to fetch static content (C4C UI-floorplans, images, JS etc) from the server, the end to end response times could be 4-5x or more slower than normal. During the “warming” process, whenever a user visits the workcenters/screens the static content on that page is cached. Normal these cached objects will stay as long as a year until the cache gets “dirty” or invalidated. This can happen after a hotfix or an upgrade. However, explicitly deleting the cache can have detrimental affect on the end to end performance.

Therefore if you have enabled settings in the browser that restricts or clears the cache then could get significantly slower response times. This also represents a cold browser interaction and can be monitored from the in-tenant reports. Please check the “identifying and troubleshooting” sections for more details. 

	Recommended browser settings
	How to identify cold browsers
#2 Client Hardware or Virtualization
Client hardware such as the processor and the amount of memory available can be a significant factor in performance. Slow disk systems can also slow down the client as static assets are retrieved from the browser are not rendered quickly. When working in virtualized environments such as Citrix, please make sure that sufficient resources such as disk space are allocated for the users. If browser cache is set to delete on logoff then each time the user logs in, there will be significant slowdown.

In browser dev tool, this can easily be detected by long SEND times or BLOCKED states.

	How to troubleshoot high rendering times
#3 Workflow Rules
Workflow rules can add to the server time if you have too many of them or you have written logic that adds quite a bit of overhead. Also, workflow rules with timing set to “On Create/On Every Create” are triggered synchronously and blocks the UI until the logic finishes running.

Workflow time can be monitored by check the “workflow time” in the performance historical data. For more details please see the server section in the “identifying and troubleshooting” guide below. 

	Workflow best practices
#4 Reports Design
The design of a a report plays an important role. Reports are significantly impacted by the way data source and/or reports are modeled. All the rules for good SQL applies here. If you make outer joins on a field which has a large number of entries then your reports performance can get slow. There are several best practices rules in the “identifying and troubleshooting” guide below. 
	How to troubleshoot sub-optimal reports performance
#5 Home Page Design
As of 1602, the home page design alerts you with a warning if you have too many tiles and reports on the home page. Also, please make sure if you really need the home page as the landing page since this can cause a long login process. Please refer to the“identifying and troubleshooting” guide below.  
#6 UI Customizations
When customizations are made to the UI screens, additional roundtrips could be triggered. Although many components are now loaded asynchronously there are some which still loads synchronously and this can cause slowness.
#7 Cloud Application Development Studio (PDI)
The best place to check for best practices recommendations here is to check the “Solution Diagnostics” tool under the Administrator workcenter, in the system administration section. Here you would find valuable feedback in your PDI implementation. Also please check the “identifying and troubleshooting” guide below. 
	SAP Cloud Application Studio Performance Best Practices
#8 External Synchronous Web Service Calls
Synchronous web service that invoke external business systems can add an overhead if the providing business system is slow. Monitoring the WS Time in the performance historical data is a good practice. Please use tools such as SOAP UI etc to determine the performance of the external web service.
#9 Cloud Application Studio Deployments or KUT Activations
Cloud SDK deployments and KUT activations can invalidate the cache. Therefore a new copy has to be retrieved over the network slowing down performance. As of 1702, UI-floorplan metadata is pre-fetched during login process. Therefore a good practice is to activate KUT changelists via content transfer during off business hours. Therefore please follow best practices for deploying the same. More information is available in the  “identifying and troubleshooting” guide below. 
	How-to measure performance impact due to multiple Embedded Components in a facet/tab
#10 Customer Network and the Internet
Network plays a very important part in performance. Latency & bandwidth are two important aspects in getting a good end to end response. In the customer’s environment, landscape components such as proxy, web filtering etc can play an important role as well. Also internet conditions keeps changing therefore tools such as browser dev tools, traceroute etc can be very handy in troubleshooting the same.  

Your best friend here is the in-tenant diagnostics tool accessible by url.

	Network diagnostics tool: Please replace “123456” with your own tenant number. As long as latency is below 250ms and bandwidth greater than 1.5mbps you should have good response times.
 

 

Best Practices Video


Link to Presentation (slide#10 onwards) describes Top 10 best practices

Identifying and Troubleshooting Performance Issues
The following blogs and links provide helpful links in understanding, troubleshooting and following best practices in performance.

		
Client Overview
 Best Practices and How-Tos
⚡ Recommended Browser Settings for SAP Hybris Cloud for Customer
How to identify cold browser interactions
How to monitor cold browser interactions using in-tenant reports
How to troubleshoot high rendering time on the browser client
 

 

Network Overview
        Tools and Best Practices
⚡ HowTo troubleshoot network issues Overview
Identify bandwidth and latency issues using in-tenant tool
How to check proper DNS resolution based in geolocation for SAP Cloud for Customer
How to identify potential network problems in C4C using HTTP Watch
How to validate routing of last mile in the context of SAP Cloud for Customer
Server Overview
Best Practices and How-Tos
⚡ How to analyze impacts on server time due to sub-optimal customizations 
How to troubleshoot sub-optimal reports performance
How to take HTTP Traces using browser DevTools


Best viewed in full screen

Cloud for Service Performance Best Practices
Optimize the performance of your Cloud for Service implementation

(Link) Part 1

In customer service every second matters.  Angry customers don’t like to wait, and agents must to be able to address their questions promptly.  In order to deliver great customer service, agents should be supported by a well tuned system, optimized to provide the best experience with minimal response times

(Link) Part 2

Customer Service agents are constantly working on a ticket after the other.  The time it takes for the ticket UI to open, and the time it takes to save a ticket after a change, are usually the two most important factors in determining the performance experienced by agents.  We will review several ways to optimize the Save step in Part 2.  In this section, let’s review a few ways in which you can optimize the time to Open.

For questions and suggestions please leave a comment
