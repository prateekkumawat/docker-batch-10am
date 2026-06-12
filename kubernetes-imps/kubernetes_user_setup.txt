# generate key for kubernetes normal user 
openssl genrsa -out kube1user.key 2048

# generate csr for kubernetes normal user 
openssl req -new -key kube1user.key -out kube1user.csr -subj "/CN=kube1user/O=development"

# validate and generate crt for normal user 
openssl x509 -req -in kube1user.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out kube1user.crt -days 365

# set kubernetes credentials on your user 
kubectl config set-credentials kube1user --client-certificate=/home/ec2-user/prateek/kube1user.crt --client-key=/home/ec2-user/prateek/kube1user.key

# set user context in kubernetes files 
kubectl config set-context kube1user-context --cluster=kubernetes --namespace=default --user=kube1user

# validate user in kubernetes 
kubectl config view
kubectl config get-contexts

# create a clusterRole or role for kuberenetes user 

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: role-developer
  namespace: default
rules:
- apiGroups: ["", "extensions", "apps"]
  #
  # at the HTTP level, the name of the resource for accessing Secret
  # objects is "secrets"
  resources: ["pods", "deployments", "replicasets"]
  verbs: ["get", "watch", "list", "create", "delete"]



# role bind for created user 
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: role-bind-developer
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: User
  name: kube1user
  apiGroup: ""
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: ClusterRole #this must be Role or ClusterRole
  name: role-developer # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: ""


# check user is working properly or not 
kubectl --context=kube1user-context get pod 