# testkube-policies

## validate-image-tags

The policy checks for image tags and throws a warning if `latest` tag is used.
Deploy policy on cluster and then apply the Test Workflow to check for compliance.

```
$ kubectl apply -f validate-image-tags.yaml 
Warning: system:serviceaccount:kyverno:kyverno-reports-controller requires permissions get,list,watch for resource TestWorkflow
clusterpolicy.kyverno.io/require-image-tags created

$ kubectl apply -f versioned-image-jmeter-tw.yaml 
testworkflow.testworkflows.testkube.io/jmeter-sample-version created

$ kubectl apply -f latest-image-jmeter-tw.yaml 
Warning: policy require-image-tags.validate-image-tags: validation failure: validation error: Using 'latest' tag is not allowed. Please specify a version. rule validate-image-tags failed at path /container/image/
testworkflow.testworkflows.testkube.io/jmeter-sample-latest created

```

## mutate-custom-label

The policy adds custom label if it is missing.

Deploy policy on cluster and apply the Test Workflow to check for compliance.

```
$ kubectl apply -f mutate-custom-label.yaml 
Warning: system:serviceaccount:kyverno:kyverno-reports-controller requires permissions get,list,watch for resource TestWorkflow
clusterpolicy.kyverno.io/mutate-testworkflow-label created

$ kubectl apply -f tw-without-label.yaml 
testworkflow.testworkflows.testkube.io/jmeter-missing-label created
sonali@SONALI-SRIVASTAVA:~/github/testkube-policies/mutate-custom-labels$ kubectl testkube get tw jmeter-missing-label

Context: cloud (2.1.60)   Namespace: testkube   Org: SONALI SRIVASTAVA-personal-org   Env: SONALI SRIVASTAVA-personal-env
-------------------------------------------------------------------------------------------------------------------------
Test Workflow:
Name:      jmeter-missing-label
Namespace: testkube
Created:   2025-01-23 18:10:11 +0000 UTC

Labels:    docs=example, testkube.io/policy-test=kyverno

$ kubectl apply -f tw-with-label.yaml 
testworkflow.testworkflows.testkube.io/jmeter-label-exists created
sonali@SONALI-SRIVASTAVA:~/github/testkube-policies/mutate-custom-labels$ kubectl testkube get tw jmeter-label-exists

Context: cloud (2.1.60)   Namespace: testkube   Org: SONALI SRIVASTAVA-personal-org   Env: SONALI SRIVASTAVA-personal-env
-------------------------------------------------------------------------------------------------------------------------
Test Workflow:
Name:      jmeter-label-exists
Namespace: testkube
Created:   2025-01-23 18:11:10 +0000 UTC

Labels:    testkube.io/policy-test=kyverno, docs=example

```
