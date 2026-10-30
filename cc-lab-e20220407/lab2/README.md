# Cloud Lab 2 Submission — Load Balancer and Auto Scaling

Copy this template to `cc-lab-YOUR_ID/lab2/README.md` in your own repository. Follow [the instructions](lab2-instruction.md). Put your screenshots in `lab2/images/` with the file names below.

- **Student name: YI MONIROM**
- **Student ID: e20220407**
- **Learner Lab user name: YI MONIROM**
- **Date completed:**

---

## Security groups

![LoadBalancerSecurityGroup](images/01-sg-load-balancer.png)

![LaunchTemplateSecurityGroup](images/02-sg-launch-template.png)

## Target group

![LoadBalancerTargetGroup](images/03-target-group.png)

![Health checks](images/04-health-checks.png)

## Application Load Balancer

![Load balancer list](images/05-load-balancer-list.png)

![Load balancer details](images/06-load-balancer-details.png)

## Launch template

![AutoScalingLaunchTemplate](images/07-launch-template.png)

## Auto Scaling group

![Auto Scaling groups](images/08-asg-list.png)

![Auto Scaling group details](images/09-asg-details.png)

## Load balancing test

![First request](images/10-browser-1.png)

![Second request, different host](images/11-browser-2.png)

## Simulated application failure

![Stopping httpd](images/12-stop-httpd.png)

![Target unhealthy or draining](images/13-target-unhealthy.png)

![New target healthy](images/14-target-replaced.png)

About how many minutes passed between stopping `httpd` and the new instance becoming healthy?

---

## Questions

1. Why does the page show a different host name when you refresh it?
   - The Application Load Balancer distributes incoming requests across the healthy instances via round-robin, and the page displays the hostname of the specific instance handling that request.

2. After you stopped `httpd`, the instance was still *running* in EC2, but the target group marked it *unhealthy*. What did the health check test, and why did the instance fail it?
   - The health check tested for an HTTP 200 response on port 80 (`/`). It failed because stopping `httpd` shut down the web server, so the instance could no longer answer HTTP requests.

3. Who replaced the failed instance: the load balancer, the target group, or the Auto Scaling group? Explain the role of each one.
   - The **Auto Scaling group** replaced it.
     - **Target group:** Monitors application health via port 80 checks and reports status.
     - **Load balancer:** Stops sending traffic to unhealthy instances.
     - **Auto Scaling group:** Terminates the unhealthy instance and launches a new one to maintain desired capacity.

4. With desired 2, minimum 2 and maximum 3, how many instances run normally? In what situation could there be 3?
   - Normally **2 instances** run.
   - There could be 3 if a scaling policy triggers under high traffic/load, or temporarily while a replacement instance launches before an unhealthy one terminates.

5. The instances' security group allows HTTP from anywhere. How could you change it so that the instances accept HTTP **only** from the load balancer?
   - In the instances' security group, edit the inbound HTTP rule and set the **Source** to the **Load Balancer's Security Group ID** instead of `0.0.0.0/0`.

## Problems and how I solved them (optional)

## AI note (optional)

One suggestion an AI tool gave you and how you checked it.

## Clean up

- [x] `MyAutoScalingGroup` deleted and its instances terminated.
- [x] `AutoScalingLaunchTemplate` deleted.
- [x] `MyLoadBalancer` and `LoadBalancerTargetGroup` deleted.
- [x] Both security groups deleted.
