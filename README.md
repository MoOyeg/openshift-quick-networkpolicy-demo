# openshift-quick-networkpolicy-demo

## Quick Demo to help show how network policies work in Openshift/Kubernetes

### Steps To Run

- Clone this repo  

- Change Directory into Repo
  - ```cd ./openshift-quick-networkpolicy-demo```
  
- Build the Container image for demo using Red Hat UBI Base Image

  - ```oc create -k ./image-build```

- Create Pods and Service to test policies

  - ```oc create -k ./policy-tests```

- Wait for pods to become ready on namespace-a and namespace-b

- Get logs from reciever pod, we should see traffic from senders in namespace-a and namespace-b
  - ```oc logs --tail=20 $(oc get pod -n namespace-a -l app=ubi8-nc-reciever -o name) -n namespace-a```  
  
### Test-1 : Block Traffic from any namespace into namespace-a

- Create networkpolicy  
  - ```oc create -f ./policy-tests/policy-same-namespace-only.yaml```
  
- After a few seconds get logs from reciever, we should only see traffic from namespace-a
  
  - ```oc logs --tail=20 $(oc get pod -n namespace-a -l app=ubi8-nc-reciever -o name) -n namespace-a```

- Delete NetworkPolicy to allow traffic  

  - ```oc delete networkpolicy/allow-same-namespace -n namespace-a```
  
- Get logs from reciever pod, we should see traffic from senders in namespace-a and namespace-b
  - ```oc logs --tail=20 $(oc get pod -n namespace-a -l app=ubi8-nc-reciever -o name) -n namespace-a```  

### Test-2 : Allow only traffic from sender pod in namespace-b

- Create networkpolicy  
  - ```oc create -f ./policy-tests/policy-only-pod-selector.yaml```
  
- After a few seconds get logs from reciever, we should only see traffic from namespace-a
  
  - ```oc logs --tail=20 $(oc get pod -n namespace-a -l app=ubi8-nc-reciever -o name) -n namespace-a```

- Delete NetworkPolicy to allow traffic  

  - ```oc delete networkpolicy/policy-only-pod-selector -n namespace-a```
  
- Get logs from reciever pod, we should see traffic from senders in namespace-a and namespace-b
  - ```oc logs --tail=20 $(oc get pod -n namespace-a -l app=ubi8-nc-reciever -o name) -n namespace-a```  

### Clean Up

```oc delete project/quick-network-policy-images && oc delete project namespace-a  && oc delete project namespace-b```
  