# GitOps Workshop: Your First Argo CD Application

In this exercise, you will create an Application using the Argo CD web interface and deploy it to Kubernetes. You can then try out how changes in Git are applied automatically.

## Important Links

- **Slides:** [Open SharePoint](https://viadee-my.sharepoint.com/:f:/g/personal/michael_biech_viadee_de/IgCuqGO-shrFT4c7SKQhcjt_ATnnG_iF0N_qrhHEK238l-Y?e=yGBlQQ)
- **Argo CD:** https://argocd.k8s-schulung.viadee.cloud/
- **Exercise repository:** https://github.com/max-bastin/gitops-workshop-demo

## Preparation

> Replace `YOUR_USERNAME` everywhere with your assigned exercise username. Use only your own branch and namespace. Neither a local installation nor `kubectl` is required for the basic exercise.

## 1. Create an Application

Open [Argo CD](https://argocd.k8s-schulung.viadee.cloud/), log in with the provided credentials, and click **+ NEW APP**.

Enter the following values:

| Section | Field | Value |
| --- | --- | --- |
| General | Application Name | `demo-app-YOUR_USERNAME` |
| General | Project Name | `demo` |
| General | Sync Policy | `Manual` |
| Source | Repository URL | `https://github.com/max-bastin/gitops-workshop-demo` |
| Source | Revision / Branch | `YOUR_USERNAME` |
| Source | Path | `demo-app/dev` |
| Destination | Cluster | `https://kubernetes.default.svc` |
| Destination | Namespace | `YOUR_USERNAME` |


Then click **CREATE**.

## 2. Synchronize the Deployment

1. Open your Application `demo-app-YOUR_USERNAME`.
2. Look at the resource tree and the sync status. Before the first deployment, the Application is typically **OutOfSync**: The desired state from Git is not yet present in the cluster.
3. Click **SYNC → SYNCHRONIZE**.
4. Watch as the Kubernetes resources are created and the pods start.

**Goal:** The Application shows **Synced** and, after starting successfully, **Healthy**.

| Status | Meaning |
| --- | --- |
| OutOfSync | Git and the cluster differ. |
| Synced | The cluster matches the desired manifests from Git. |
| Progressing | Resources are still being set up. |
| Healthy | The resources are ready to use. |
| Degraded | At least one resource reports a problem. |

Sync status and health are independent: **Synced** alone does not mean that the application is running successfully.

## 3. Bonus Task: Observe Auto-Sync

1. In your Application, open **APP DETAILS → SYNC POLICY** and enable automatic synchronization.
2. Open the [exercise repository on GitHub](https://github.com/max-bastin/gitops-workshop-demo) and switch to your branch `YOUR_USERNAME`.
3. Find the replica count of the demo deployment in the configuration under `demo-app/dev`. Increase the existing value, for example from `1` to `3`:

   ```yaml
   replicas: 3
   ```

4. Commit the change to **your branch**, for example with the message:

   ```text
   Increase demo replicas to 3
   ```

5. Switch back to Argo CD. Wait until the change is detected, or click **REFRESH** to trigger the comparison. This time, **do not click SYNC**.
6. Observe the automatic synchronization and the additional pod replica in the resource tree.

**Expected result:** Argo CD applies the change from Git without a manual sync. In the end, the Application is **Synced** and **Healthy** again.