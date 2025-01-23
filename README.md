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
