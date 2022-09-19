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

### Deployment Manifest
- Deployment is a kubernetes construct
- Need to abstract cloud specific construct in K8s construct
- enables k8s to run in multiple platform
- say hello to service account

ALB/EC2 policies-> attached to a role -> attached to a Service account -> attached to deployment manifest
service account/IAM role, exists in Amazon EKS --> need to have a way to communicate with AWS Service
to validate the policies and temp creds
For that, we have IAM OIDC Provider(cluster level)


![image](https://user-images.githubusercontent.com/81581601/191018041-cc5df832-2208-416c-81b4-60c8fc3a22a9.png)


When SA is not specified, default is assumed

$kubectl get sa

$kubectl get sa -A (all service accounts)

$kubectl describe sa serviceaccount -n namespace
- could see IAM role in annotations

Service account and Cluster
--------------------------------
IAM role is all about specific implementation(AWS in this case)
Pods needs Kubernetes cluster access as well
ClusterRoles grant access to cluster specific resources
 - create/delete/List( Nodes, Pods, Namespaces) etc
 
 $kubectl get clusterrole
 
 $kubectl describe clusterrole admin -> have access to all k8s resources

 
For your application, have limited accesses

kind: ClusterRole , kind: ServiceAccount --> create both tie them both with ClusterRoleBinding

kind:ClusterRoleBinding

ClusterRole is reusable

Role Vs ClusterRole
---------------------
Role is K8s role

Both Role and ClusterRole represent set of pe                      rmissions

A Role sets permission with a specific namespace - have to specify ns
ClusterRole is non-namespaced

Kubernetes security
---------------------
- Your Application
- You!(as human user)

![image](https://user-images.githubusercontent.com/81581601/191064279-1a5f06b1-f30a-42a8-b8e3-639242367042.png)
                                                                                                                                          
Controlling access through Roles,ClusterRoles,ClusterRole binding is called Role Based Access Control(RBAC)

Benefits:
- Each application can have different access
- Node IAM role does nt have to have all access
- Secular and granular design
- Also known as ISRA(IAM Roles for service accounts) - replaces kube2iam
- 




