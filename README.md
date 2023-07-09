- 👋 Hi, I’m @dvhii
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
 mkdir -p "$dir"
cd "$dir" || exit

# Get all deployments
deployments=$(kubectl get deployments -n united-manufacturing-hub -o name)
# Get all statefulsets
statefulsets=$(kubectl get statefulsets -n united-manufacturing-hub -o name)
# Get all jobs
jobs=$(kubectl get jobs -n united-manufacturing-hub -o name)

# Get logs for all deployments
for deployment in $deployments; do
    log="${deployment##*/}"
    log="${log//*united-manufacturing-hub-}"
    kubectl logs "$deployment" -n united-manufacturing-hub --all-containers --prefix > deployment-"$log".log || true
done

# Get logs for all statefulsets
for statefulset in $statefulsets; do
    log="${statefulset##*/}"
    log="${log//*united-manufacturing-hub-}"
    kubectl logs "$statefulset" -n united-manufacturing-hub --all-containers --prefix > statefulset-"$log".log || true
done

# Get logs for all jobs
for job in $jobs; do
    log="${job##*/}"
    log="${log//*united-manufacturing-hub-}"
    kubectl logs "$job" -n united-manufacturing-hub --all-containers --prefix > job-"$log".log || true
done

# Get all pods and services
kubectl get po,svc -n united-manufacturing-hub > kubectl_pods_and_services.log

# Get all events
kubectl get events -n united-manufacturing-hub > kubectl_events.log

# Describe all pods
kubectl describe po -n united-manufacturing-hub > kubectl_describe_pods.log

# Describe all services
kubectl describe svc -n united-manufacturing-hu > kubectl_describe_services.log

# Helm status
helm status united-manufacturing-hub -n united-manufacturing-hub > helm_status.log

# cd back to original directory
cd - || exit
 
<!---
dvhii/dvhii is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
