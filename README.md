An operator that allows users to provision namespaced installations of DSP within a cluster.

# Quickstart

Deploy the operator
```bash
oc new-project data-science-pipelines-operator
cd ${REPO}/config/default
kustomize build . | oc apply -f -
```

Deploy a DSP instance in a namespace
```bash
DSP_Namespace=test-ds-project-1
oc new-project ${DSP_Namespace}
cd ${REPO}/config/samples
kustomize build . | oc -n ${DSP_Namespace} apply -f -
```

Deploy DSP in another namespace: 

```bash
DSP_Namespace_2=test-ds-project-2
oc new-project ${DSP_Namespace_2}
cd ${REPO}/config/samples
kustomize build . | oc -n ${DSP_Namespace_2} apply -f -
```

Cleanup:

```bash


cd ${REPO}/config/samples
kustomize build . | oc -n ${DSP_Namespace} delete -f -
kustomize build . | oc -n ${DSP_Namespace_2} delete -f -
oc delete project ${DSP_Namespace}
oc delete project ${DSP_Namespace_2}

cd ${REPO}/config/default
kustomize build . | oc delete -f -
oc delete project data-science-pipelines-operator
```


# Run tests 

Simply clone the directry and execute `make test`.

To run it without `make` you can run the following: 
```bash
tmpFolder=$(mktemp -d)
go install sigs.k8s.io/controller-runtime/tools/setup-envtest@latest
export KUBEBUILDER_ASSETS=$(${GOPATH}/bin/setup-envtest use 1.25.0 --bin-dir ${tmpFolder}/bin -p path)
go test ./... -coverprofile cover.out

# once $KUBEBUILDER_ASSETS you can also run the full test suite successfully by running:
pre-commit run --all-files
```