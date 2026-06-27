## Topics

What the CKS exam covers:

- Cluster Security: 
    - `kube-bench` to identify security issues and apply remediations.
    - Certificates & Mutual TLS, similar to CKA.
    - Kubeconfig, RBAC, Service Accounts
    - Network Policies: both native and Cilium.
    - Kubelet Security: anonymous authentication, read-only port
- System Security
    - Host security: users, groups, permissions, SSH
    - Network firewall: `ufw`
- Pod Security:
    - Pod Security Standards & Pod Security Admission
    - Admission Controller
    - Open Policy Agent (OPA) and Gatekeeper
    - Pod to Pod encryption with Istio `PeerAuthentication`
- Container Security
    - Security Context: `readOnlyRootFilesystem`, `runAsNonRoot`, `runAsUser`, `runAsGroup`, `allowPrivilegeEscalation`.
    - Seccomp, AppArmor to restrict system calls.
    - Securing docker configuration.
    - Custom container runtimes: gVisor, Kata Containers.
- Code Security:
    - Static analysis: may be manually detect like credential exposure, or using tools like `kube-linter`
    - Generating SBOM (Software Bill of Materials): `bom`, `syft` or `trivy`.
    - Scanning Docker images for vulnerabilities with `trivy`
    - Scanning K8s yaml manifests for vulnerabilities with `kubesec`

- Monitoring, Logging, and Runtime Security:
    - Audit logs: setup `kube-apiserver` audit logs.
    - Threat Detection: writing rules for Falco with custom output, or grep Falco logs to find pod/deployment that violate security policies.


## Exam Preparation

1. Course: KodeKloud CKS course. This is the only course I took, covered all topics in the exam. KodeKloud CKS course is not available on Udemy, so I take 1 month subscription to KodeKloud platform (Standard tier, 29$/month). That also set the deadline for me to save cost, so +1 motivation.

Unlike CKA and CKAD, for CKS I jump directly to the labs without watching videos in advance. Google every concept, command or configuration encountered in the labs. Then after finishing the labs, watch the videos to fill in any gaps. For me it is more effective.

2. Mock exams: 
- KodeKloud CKS mock exams: 3 mock exams included in the course. There are some overlapping questions (30-50%), which are important because they appear often in the real exam. Try again until you reach 90% score. Note that KodeKloud does not do partial scoring, unlike the real exam, so you need to complete ALL parts of a question correctly to get the score, or 0 if you get any part wrong. Personally I think if you score >75%, you are ready for the real exam.

- Killer.sh: 2 mock exams provided for free when you purchase the real exam. Harder and longer than the real exam questions, but good to familiarize with the exam environment and scoring. And I actually enjoy doing it, some techniques in the answers are useful (in real world). I scored just 60% and 52%. 


I spent 2 weeks to prepare for CKS, immediately after finshing CKA. 1 week for KodeKloud labs, and 1 week for mock exams. 



## Tips

1. On exam day, checkin early. You can start 30 minutes before the exam start time. The checkin process usually takes 15-25 minutes.

2. Go to the toilet before, and don't drink too much. You may request a break during the exam, but the counter won't stop. 

3. In the exam, you should use 2 terminals session. Some questions requires you to edit a manifest file (`vim` or `nano`) while checking current configuration at the same time. For example, network policies questions usually require checking labels of pods, you don't want to switch in and out of `vim` editor in just 1 termimal session. 

4. Choose questions wisely. Try topics you are familiar with first. In the exam, at the top of every question is the links to relevant documentation, named after the topic. Skim that line first to identify the topics. Then, if you are familiar, read the question, and if you feel confident, answer it. Otherwise, skip.

5. Because partial scoring is applied, do as many tasks in a question as possible to maximize your score. 

6. For Pod Security Admission, easy question is just labeling namespace with `pod-security.kubernetes.io/<MODE>: <LEVEL>`. Trickier question is to inspect why pods are not created and running (due to policy violations) and fix it. You can use `kubectl get deploy/pod <name> -o yaml` and look for `.status.conditions[].message`. The answer is there and obvious. I got one question like this in the exam, and was confused at first because I did not know where to look at. `kubectl describe` does not show the event.

For example, `kubectl describe deploy` shows:

```
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  30s   deployment-controller  Scaled up replica set test-6bc97d7d78 from 0 to 1
```

Nothing useful here.

And `kubectl get deploy -o yaml` shows:
```yaml
status:
  conditions:
  - lastTransitionTime: "2026-06-26T17:28:17Z"
    lastUpdateTime: "2026-06-26T17:28:17Z"
    message: Created new replica set "test-6bc97d7d78"
    reason: NewReplicaSetCreated
    status: "True"
    type: Progressing
  - lastTransitionTime: "2026-06-26T17:28:17Z"
    lastUpdateTime: "2026-06-26T17:28:17Z"
    message: Deployment does not have minimum availability.
    reason: MinimumReplicasUnavailable
    status: "False"
    type: Available
  - lastTransitionTime: "2026-06-26T17:28:17Z"
    lastUpdateTime: "2026-06-26T17:28:17Z"
    message: 'pods "test-6bc97d7d78-s89x2" is forbidden: violates PodSecurity
      "restricted:latest": allowPrivilegeEscalation != false (container "nginx" must
      set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities
      (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot
      != true (pod or container "nginx" must set securityContext.runAsNonRoot=true),
      runAsUser=0 (container "nginx" must not set runAsUser=0), seccompProfile (pod
      or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault"
      or "Localhost")'
    reason: FailedCreate
    status: "True"
    type: ReplicaFailure
  observedGeneration: 1
  terminatingReplicas: 0
  unavailableReplicas: 1

```

The answer is obvious: just edit the deployment/pod and add corresponding `securityContext` to fix the violations.

7. Two common questions which requires editing `kube-apiserver` manifest file: enable audit logs, and enabling admission controller. It is best to have a copy of the original `kube-apiserver` manifest file, to reset the state in case you mess it up:

```sh
cp /etc/kubernetes/manifests/kube-apiserver.yaml kube-apiserver.yaml
```

And if you need to reset, just overwrite:

```sh
cp kube-apiserver.yaml /etc/kubernetes/manifests/kube-apiserver.yaml
```

For both questions, don't forget to mount the correct file/directory for audit/admission policy configurations! Practice a lot, they are common questions in the exam.


8. For Falco question, expect at least to write custom output or even rule. In the exam you will have access to Falco documentation, but better familiarize to the docs to quickly find where to look for the answer. The page I found useful are [Supported Fields for Conditions and Outputs](https://falco.org/docs/reference/rules/supported-fields/): to find the correct field name for output or condition. Use Ctrl + F to search.

Just remember how to navigate to it in the exam: Falco docs -> Reference -> Rules -> Supported Fields for Conditions and Outputs.


9. For Cilium network policy, the useful page is [Overview of Network Policy](https://docs.cilium.io/en/stable/security/policy/#overview-of-network-policy). Click on the Layer3/4 link to find sample manifest file depending on the question.



