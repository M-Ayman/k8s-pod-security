### Install OPA

    kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.1/deploy/gatekeeper.yaml

    kubectl get pods -n gatekeeper-system




### Apply All policies & constraints


    cd pss/policies

    kubectl apply -f .


### Apply All  constraints

    cd pss/constraints

    kubectl apply -f .


### Apply one by one

    cd pss/policies

    kubectl apply -f 002-Privileged.yaml

    cd pss/constraints

    kubectl apply -f 02-Privileged.yaml


### Testing

    cd pss/test/02-Privileged

    kubectl apply -f bad.yaml


### Apply MustRunAsNonRoot  to runASUser

    kubectl edit psp eks.privileged

  and change

    runAsUser:
      rule: RunAsAny
  To

    runAsUser:
      rule: MustRunAsNonRoot    



### Optimize yaml file

    apiVersion: v1
    kind: Pod
    metadata:
      name: ok-nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        securityContext:
          allowPrivilegeEscalation: false
          privileged: false
          readOnlyRootFilesystem: true
      hostNetwork: false
      hostIPC: false
      hostPID: false
