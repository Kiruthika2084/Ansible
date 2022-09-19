RBAC,IRSC, ClusterRoles, ClusterRole binding
----------------------------------------------------

POD needs access to AWS resources -either to create ALB
or attach security group to your worker nodes(EC2)

/
IN place of POD , if it is an EC2 instance, how to have the same accesses?

Attach a role which has required policies.
/
Similarly, can attach a role to POD, but pod life is not simple.
So we need to deploy in deployment manifest

Deployment is a kubernetes construct
Need to abstract cloud specific construct in K8s construct
 - enables k8s to run in multiple platform
say hello to service account

ALB/EC2 policies-> attached to a role -> attached to a Service account -> attached to deployment manifest
When SA is not used, default is assumed
$kubectl get sa

Service account and Cluster
--------------------------------
IAM role is all about specific implementation(AWS in this case)
Pods needs Kubernetes cluster access as well
ClusterRoles grant access to cluster specific resources
 - create/delete/List( Nodes, Pods, Namespaces) etc
 
 $kubectl get clusterrole
 $kubectl describe clusterrole admin -> have access to all k8s resources
 
For your application, have limited accesses

kind: ClusterRole , kind: ServiceAccount --> create both
tie them both with ClusterRoleBinding

kind:ClusterRoleBinding

ClusterRole is reusable

Role Vs ClusterRole
---------------------
Role is K8s role




