# Kubernetes – “YOLO Shopping-Cart”  
**Google Kubernetes Engine (GKE) Edition**

Live application ➜ [http://34.35.133.159:3000](http://34.35.133.159:3000)  
GitHub repository ➜ https://github.com/Vicmalash/k8s/
## Repository map
k8s/
├── namespace.yaml
├── mongo-statefulset.yaml
├── mongo-service.yaml
├── backend-deployment.yaml
├── backend-service.yaml
├── backend-pvc.yaml
├── frontend-deployment.yaml
├── frontend-service.yaml
├── frontend-pvc.yaml
├── README.md
└── explanation.md

## Deployment command
```bash
git clone https://github.com/Vicmalash/k8s.git
cd k8s
kubectl apply -f .
**Quick checks**
Pods ready	kubectl get po -n yolo
Services	kubectl get svc -n yolo
Persistent data	kubectl get pvc -n yolo
**Test the cart**
Open http://34.35.133.159:3000
Add items to cart
Delete mongo pod: kubectl delete po -l app=mongo -n yolo
Refresh page – cart still contains items (persistent volume survives).
Images used (Docker Hub)

****Component	Image
Frontend	vicmalash/yolo-client:v1.0.4
Backend	vicmalash/yolo-backend:v1.1.0
MongoDB	mongo:6.0

****Git workflow highlights
12 descriptive commits:git log --oneline

