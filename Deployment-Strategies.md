# Deployment Strategies
When deploying an app to Kubernetes there are a few common deployment strategies to be aware of

## Rolling deployment
This is the default way Kubernetes handles deployments in a cluster: it will spin one pod up, then the next, then the next, etc. For a small deployment of relatively simple application this process can be completed very quickly, but your manifests can be configured to better control how your deployment is rolled out:
- You can configure how many pods can be down at a time
- You can configure how many pods are being affected at a time
- etc

There are pros and cons to this: as a strategy it is the easiest to implement, seeing as it is used by default. It is also relatively simple to configure since you just need to adjust your deployment manifest. However, this method becomes slow and cumbersome the more pods you add and the more complex the application you are deploying. This process also becomes somewhat convoluted if you need to rollback a large deployment that uses the rolling strategy, because your rollback will also use the rolling strategy, which for large deployments can take a significant amount of time. 

## Blue/Green Deployment
In this strategy the "current" deployment is called the "blue" deployment, and the new version of the deployment you want to switch to is called the "green" deployment. Using the Blue/Green strategy you would set up the green deployment alongside the blue one, and once you confirm all pods/containers are running you redirect traffic from the blue deployment to the green deployment (without shutting down the blue deployment just yet). You then closely monitor the green deployment to ensure that your SLIs are still good and that you don't have any weird situations happening. If something goes wrong (requests are failing, traffic is not reaching the green pods, etc) you simply undo your changes to the routing rules (probably going to be undoing changes to your ingress) and traffic is sent back to the original blue deployment, and you have time to work with the dev team or whoever else to figure out what went wrong with the green deployment. You can continue this process with very little downtime/error budget usage because you always have a working version of your deployment to revert back to with very little required on your part to do so. Once the green deployment is considered a success you can start to consider it as your new "blue" deployment and you are free to destroy the old one and stop using those resources (like vms needed to maintain those pods can be shut off)

- Pros
    - Maintains uptime very well
    - It is relatively resource un-intensive to switch traffic from one deployment to another if something goes wrong with the green deployment
- Cons
    - For large deployments this strategy can incur serious monetary costs for maintaining two deployments for an extended amount of time (need to pay for vms, memory, cpu, etc)
    - If you delete your old deployment too quickly, it can take a long time to get everything back up and running

## Canary Deployment
With this strategy instead of setting up an entire new deployment like in the Blue/Green system you instead set up a single pod with the updated application, and you direct a small amount of traffic towards it. If your metrics are good, you slowly change the pods of your primary deployment to match the canary deployment until all of them have been updated. If something goes wrong, you just revert the individual pods back to their previous version, make whatever changes are necessary, and then try again.

- Pros
    - There is much less of a monetary cost than the Blue/Green Strategy since you are not recreating an entire deployment
    - Can make use of beta testers with an isolated canary pod to get more feedback on the update
    - If things go wrong because you are manually changing things it is relatively quick to revert back to your old deployment
- Cons
    - The hands on approach can take time away from other important work (more manual work than blue/green strategy)
    - It is still slower to roll back updates than the blue/green strategy due to manually making changes to all affected pods
    - If you aren't careful/consistent with how you use your canary deployment you can surprise users with features they were not expecting