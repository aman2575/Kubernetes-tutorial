### Rolling Update in DaemonSets and StatfulSets

- StatefulSets, have only one options to configure the rollout, Pods are managed in order byb the statfulSets, which also applies to updates - the rollouts proceeds backward from the last Pod in the set down to the first.

- There's no maxSurge or maxUnavailable setting for StatfulSets. The update is always by one pod at a time. 

- Your configuration option is to define how many Pods should be updated in total, using the partition setting. This setting define the cutoff point where the rollout stops, and it;s useful for performing a staged rollout of a statful app.


## 9.5 Understanding release strategies

- Blue-Green Deployment
    - You can have both the old and new version of your ap deployed at the same time, but only one version is active. You can flip a switch to choose which version is the active one. You can do that by updating the label selector in a Service to send traffic to the Pods in a different Deployment.