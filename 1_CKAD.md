## Topics

- Pod, Deployment, Job, CronJob
- Service, Ingress, Network Policy
- ConfigMap, Secret, Security Context, Service Account, RBAC
- Volumes, PersistentVolume, PersistentVolumeClaim


## Exam Preparation


1. Course: I only take KodeKloud CKAD course on Udemy. One-time purchase, lifetime access. Try to save cost by finding discount codes. Videos and labs are helpful. If you have no prior Kubernetes experience, I would recommend to watch the videos first, then do the labs.  If you have some experience, you can jump directly to the labs, and watch the videos to fill in any gaps.

KodeKloud Lightning Labs: 2 labs which requires setting up K8s components to build a small system. Much longer than a question in the real exam, but very interesting. Highly recommend to do it to get a better sense of how each K8s component works together. 

It took me 4 weeks to finish the course.

2. Mock exams:

- KodeKloud mock exams: 2 mocks. Easy if you finished the lightning labs. 

- Killer.sh: 2 sessions but same question set, unlike CKA and CKS. Longer and harder than the real exam, but good to familiarize with the exam environment and scoring. I actually enjoy doing it, some techniques in the answers are useful (in real world).

1 week to do and review. CKAD is the easiest among the 3, just **know how to do things**, not *troubleshoot and fix things* like CKA and CKS. 


## Tips

1. On exam day, checkin early. You can start 30 minutes before the exam start time. The checkin process usually takes 15-25 minutes.

2. Go to the toilet before, and don't drink too much. You may request a break during the exam, but the counter won't stop. 

3. Choose questions wisely. Try topics you are familiar with first. In the exam, at the top of every question is the links to relevant documentation, named after the topic. Skim that line first to identify the topics. Then, if you are familiar, read the question, and if you feel confident, answer it. Otherwise, skip.

4. Because partial scoring is applied, do as many tasks in a question as possible to maximize your score.

5. **Memorize imperative commands**! It saves you tons of time. For example:



```sh
k -n <namespace> run pod <pod-name> --image=<image-name> 
k -n <namespace> create deploy <deploy-name> --image=<image-name>
k -n <namespace> expose deploy <deploy-name> --port=<port> --target-port=<target-port> --name=<service-name> --type=<service-type>
k -n <namespace> expose pod <pod-name> --port=<port> --target-port=<target-port> --name=<service-name> --type=<service-type>
```

Some configurations cannot be set by imperative commands. In such cases, use **`dry-run=client -o yaml > file-name.yaml`** to generate the manifest file, then edit it with `vim`. 


Best to use `-n <namespace>` immediately after `kubectl`. Many errors come from wrong namespace. 

6. Some `vim` tips:

- `dd` to delete the current line in normal mode, `d2d` to delete 2 lines, `d3d` to delete 3 lines, etc. `d$` to delete from the cursor to the end of the line. 
- To **indent multiple lines** (very useful when you copy paste `pod` yaml from the K8s docs to a `deployment` pod template): first `ESC` to go to normal mode, jump to the first line, then `Ctrl+v` to go to visual block mode, use `j` or down arrow to select multiple lines, then:
    - `Shift >` to indent right. `2 Shift >` to indent twice, `3 Shift  >` to indent 3 spaces right, etc.
    - `Shift <` to indent left, `2 Shift <` to indent twice, `3 Shift <` to indent 3 spaces left, etc.

7. Open a 2nd terminal for checking information while editing a manifest file in the 1st terminal. For example, use it for `k explain` to search and copy paste the correct field names, or `k get --show-labels` if you need label configuration.




