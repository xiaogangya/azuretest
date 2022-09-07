Resource|Free|Shared (Preview)|Basic|Standard|Premium (Preview)</th>
---|---|---|---|---|---
[Web, mobile, or API apps](../services/app-service/) per [App Service plan](web-sites-web-hosting-plan-overview.md)<sup>1</sup>|10|100|Unlimited<sup>2</sup>|Unlimited<sup>2</sup>|Unlimited<sup>2</sup>
[Logic apps](../services/app-service/) per [App Service plan](web-sites-web-hosting-plan-overview.md)</a><sup>1</sup>|10|10|10|20 per core|20 per core 
[App Service plan](web-sites-web-hosting-plan-overview.md)|1 per region|10 per resource group|10 per resource group|10 per resource group|10 per resource group
Compute instance type|Shared|Shared|Dedicated<sup>3</sup>|Dedicated<sup>3</sup>|Dedicated<sup>3</sup></p>
[Scale-Out](web-sites-scale.md) (max instances)|1 shared|1 shared|3 dedicated<sup>3</sup>|10 dedicated<sup>3</sup>|50 dedicated<sup>3,4</sup>
Storage<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU time (day)<sup>6</sup>|60 minutes|240 minutes|Unlimited, pay at standard [rates](../pricing/details/app-service/)</a>|Unlimited, pay at standard rates|Unlimited, pay at standard rates
Memory (1 hour)|1024 MB per App Service plan|1024 MB per app|N/A|N/A|N/A
Bandwidth|165 MB|Unlimited, [data transfer rates](../pricing/details/data-transfers/) apply|Unlimited, data transfer rates apply|Unlimited, data transfer rates apply|Unlimited, data transfer rates apply
Application architecture|32-bit|32-bit|32-bit/64-bit|32-bit/64-bit|32-bit/64-bit
Web Sockets per instance<sup>7</sup>|5|35|350|Unlimited|Unlimited
Concurrent [debugger connections](web-sites-dotnet-troubleshoot-visual-studio.md) per application|1|1|1|5|5
[azurewebsites.net subdomain with FTP/S and SSL](web-sites-configure-ssl-certificate.md)|X|X|X|X|X
[Custom domain](web-sites-custom-domain-name.md) support||X|X|X|X
Custom domain [SSL support](web-sites-configure-ssl-certificate.md)|||Unlimited|Unlimited, 5 SNI SSL and 1 IP SSL connections included|Unlimited, 5 SNI SSL and 1 IP SSL connections included
Integrated Load Balancer||X|X|X|X
[Always On](web-sites-configure.md)|||X|X|X
[Scheduled Backups](web-sites-backup.md)||||Once per day|Once every 5 minutes<sup>8</sup>
[Auto Scale](web-sites-scale.md)|||X|X|X
[WebJobs](web-sites-create-web-jobs.md)<sup>9</sup>|X|X|X|X|X
[Azure Scheduler](../services/scheduler/) support||X|X|X|X
[Endpoint monitoring](web-sites-monitor.md)|||X|X|X
[Staging Slots (Preview)](web-sites-staged-publishing.md)||||5|20
Custom domains per app</a>||500|500|500|500
SLA||<p>|99.9%|99.95%<sup>10</sup>|99.95%<sup>10</sup>

<sup>1</sup>Apps and storage quotas are per App Service plan unless noted otherwise.  
<sup>2</sup>The actual number of apps that you can host on these machines depends on the activity of the apps, the size of the machine instances, and the corresponding resource utilization.  
<sup>3</sup>Dedicated instances can be of different sizes. See [App Service Pricing](/pricing/details/app-service/) for more details. Additional instances are available by opening a support request.  
<sup>4</sup>Premium tier allows up to 50 computes instances (subject to availability) and 500 GB of disk space when using App Service Environments, and 20 compute instances and 250 GB storage otherwise.  
<sup>5</sup>The storage limit is the total content size across all apps in the 
same App Service plan. Storage limits can be increased by opening a support request.  
<sup>6</sup>These resources are constrained by physical resources on the dedicated instances (the instance size and the number of instances).  
<sup>7</sup>If you scale an app in the Basic tier to two instances, you have 350 concurrent connections for each of the two instances.  
<sup>8</sup>Premium tier allows backup intervals down up to every 5 minutes when using App Service Environments, and 50 times per day otherwise.  
<sup>9</sup>Run custom executables and/or scripts on demand, on a schedule, or continuously as a background task within your App Service instance. Always On is required for continuous WebJobs execution. Azure Scheduler Free or Standard is required for scheduled WebJobs.  
<sup>10</sup>SLA of 99.95% provided for deployments that use multiple instances with Azure Traffic Manager configured for failover.  
