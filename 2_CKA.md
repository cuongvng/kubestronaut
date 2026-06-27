## Topics

Around 60% overlap with CKAD. Also covers:
- Troubleshooting: `kubelet`, `kube-apiserver`, `etcd`, `kube-controller-manager`, `kube-scheduler`, or application failure (ImagePullBackOff, CrashLoopBackOff, etc.)
- Cluster maintenance: upgrade with `kubeadm`, backup and restore from `etcd`, etc.
- TLS, Certificate, DNS
- DaemonSet, Static Pod, HorizontalPodAutoscaler
- Gateway API
- Helm and Kustomize



## Exam Preparation

1. Course: KodeKloud CKA course on Udemy, like CKAD.

Try to setup a small cluster yourself with `kubeadm`. I use [Multipass](https://canonical.com/multipass) to create Ubuntu VMs on Mac, then follow the [K8s docs to setup a cluster](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/).


2. Mock exams:

- KodeKloud mock exams: 3 mocks. On the same level as the real exam. 

- Killer.sh: 2 sessions. Again, longer and harder than the real exam, but good to familiarize with the exam environment and scoring. And I actually enjoy doing it, some techniques in the answers are useful (in real world).


I spent a month to prepare for CKA, 3 weeks for course/labs, and 1 week for mock exams, immediately after finishing CKAD.



## Tips

1. On exam day, checkin early. You can start 30 minutes before the exam start time. The checkin process usually takes 15-25 minutes.

2. Go to the toilet before, and don't drink too much. You may request a break during the exam, but the counter won't stop. 

3. In the exam, you should use 2 terminals session. Some questions requires you to edit a manifest file (`vim` or `nano`) while checking current configuration at the same time. For example, network policies questions usually require checking labels of pods, you don't want to switch in and out of `vim` editor in just 1 termimal session. 

4. Choose questions wisely. Try topics you are familiar with first. In the exam, at the top of every question is the links to relevant documentation, named after the topic. Skim that line first to identify the topics. Then, if you are familiar, read the question, and if you feel confident, answer it. Otherwise, skip. Personally, I would do any trobleshooting questions LAST, because cannot expect how much time it will take just by reading the question (basically it just ask there is a problem with kubelet/kube-apiserver/etcd, fix it). 

5. Because partial scoring is applied, do as many tasks in a question as possible to maximize your score. 


6. Again, imperative commands are your best friend. 

```sh
k -n <namespace> create sa <name> -n <namespace>
k -n <namespace> create role <name> --verb=<verb> --resource=<resource>
k -n <namespace> create rolebinding <name> --role=<role-name> --serviceaccount=<namespace>:<sa-name>
k create clusterrole <name>
k create clusterrolebinding <name> --clusterrole=<clusterrole-name> --serviceaccount=<namespace>:<sa-name>
k -n <namespace> auth can-i create pod --as=<user-or-sa-name> -n <namespace>

---
k -n <namespace> create configmap <name> --from-literal=<key>=<value> 
k -n <namespace> create secret generic <name> --from-literal=<key>=<value>
```

7. Practice Helm and Kustomize decently. I failed one Helm question in the exam. 
