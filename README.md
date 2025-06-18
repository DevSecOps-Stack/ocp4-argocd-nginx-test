# Fastest GUI path

Console → ☸ Administrator → Operators → OperatorHub.

1. **Search** “OpenShift GitOps”, click the **Red Hat** tile, and select **Install**.
2. **Installation mode:** “All namespaces” (cluster‑wide).
3. **Approval:** “Automatic”.

---

## Deploy a sample application with Argo CD

After the GitOps operator finishes installing, bootstrap an example Nginx app into the cluster.

1. **Create the application namespace (once):**

   ```bash
   oc create namespace nginx-argo-test
   ```

2. **Apply the Argo CD `Application` manifest:**

   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Application
   metadata:
     name: nginx-test
     namespace: openshift-gitops     # Argo CD control plane namespace
   spec:
     destination:
       namespace: nginx-argo-test
       server: https://kubernetes.default.svc
     source:
       repoURL: https://github.com/DevSecOps-Stack/ocp4-argocd-nginx-test.git
       path: .
       targetRevision: HEAD
     project: default
     syncPolicy:
       automated: {}
   ```

   ```bash
   oc apply -f nginx-app.yaml
   ```

3. **Watch it sync:**
   Open **Argo CD → Applications → nginx-test** and verify the status turns **Healthy/Synced**.

You now have a fully automated, Git‑driven deployment of a simple Nginx service that you can swap out with your own repository later.
