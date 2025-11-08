
# Implementation Reasoning 

## 1. Choice of Kubernetes Objects
- **MongoDB** → `StatefulSet`  
  - Requires *stable network identity* (`mongo.mongo.yolo.svc.cluster.local`)  
  - Needs *persistent volume* that survives pod reschedule  
  - Ordered deployment not strictly required, but `volumeClaimTemplates` is only available in StatefulSets

- **Frontend & Backend** → `Deployment`  
  - Stateless pods – no need for sticky identity  
  - Rolling updates handled natively by Deployment controller

- **Controllers**  
  - Deployments guarantee desired replica count & self-healing  
  - StatefulSet guarantees volume re-attachment to the same ordinal pod

## 2. Exposure to Internet Traffic
- **Frontend**  
  - `type: LoadBalancer` – GKE provisions external IP `34.35.133.159:3000`  

- **MongoDB**  
  - `ClusterIP: None` (headless) – only reachable inside cluster, no external exposure

## 3. Persistent Storage
- **MongoDB**  
  - `volumeClaimTemplates` creates one 2-GiB PV per replica  
  - Deleting `mongo` pod → new pod re-attaches **same volume** → cart data intact

- **Backend uploads**  
  - `backend-pvc` mounted at `/usr/src/app/uploads` – survives pod restarts

- **Frontend (optional)**  
  - `frontend-pvc` reserved for future file uploads or SSR cache

## 4. Git Workflow
- 12 commits ≥ minimum 10 required  
- Pull-requests reviewed before merge to `master`

## 5. Debugging Measures Applied
- **Events & logs** inspected with `kubectl describe` while troubleshooting

## 6. Image Tagging Best-Practice
- Format: `username/image:component-v{semver}`  
- Examples:  
  - `vicmalash/yolo-client:v1.0.4`  
  - `vicmalash/yolo-backend:v1.1.0`  
