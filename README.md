# hmi-demo

Description: This is a demo of a customer deployment of Inductive Automation Ignition application to RHEL Edge device including some of the external services required by the customer for security, logging and certification so the application can run in HTTPS mode in a completely disconnected environment.

![architecture_image](images/hmi-demo.png)

## Steps to install

1. Install OpenShift Gitops so we can use it to deploy Ansible Automation Platform.

    ```shell
    oc apply -k https://github.com/redhat-cop/gitops-catalog/openshift-gitops-operator/overlays/stable
    ```

2. Create service account, deploy the AAP operator, patch operator, controller and configurations for the URL using the patch operator to fully install Ansible Automation Platform via Openshift Gitops.

    ```shell
    oc create -f gitops/argocd/apps/aap-operator.yaml || oc create -f gitops/argocd/apps/aap-controller.yaml || oc create -f gitops/operators/patch-operator/subscription.yaml || oc create -f gitops/operators/patch-operator/rbac.yaml || oc create -f gitops/operators/patch-operator/operatorgroup.yaml || oc create -f gitops/argocd/apps/patch-operator.yaml || oc create -f gitops/aap/consolelink-patch.yaml || oc create -f gitops/aap/consolelink.yaml
    ```

3. Retrieve web address and credentials to log into the Ansible Automation Platform console, and login.

    ```shell
    oc get routes -n ansible-automation-platform | awk {'print $2'} && oc get secrets/controller-admin-password -n ansible-automation-platform -o json | jq '.data' |grep -v '{' |grep -v '}'
    ```

    Example Output:

    ```shell
    HOST/PORT
    controller-ansible-automation-platform.apps.clustername
    "password": "Z29BUzZSUFhtd0RneHZ6TnFYZVRocnlXVHY1VnJueDc="
    ```

4. Create OpenShift Gitops application to install an instance of Ansible Automation Platform Controller.


5. Log in to controller in a web browser via the address in oc get route in the HOST/PORT section

    ```shell
    oc get route -n ansible-automation-platform
    NAME       HOST/PORT                                                                      PATH   SERVICES           PORT   TERMINATION     WILDCARD
    hmi-demo   hmi-demo-ansible-automation-platform.apps.brooklyn.demo.red-chesterfield.com          hmi-demo-service   http   edge/Redirect   None
    ```

6. Once logged in you will be prompted for your console.redhat.com credentials to choose which subscription to use.

## Configuring Ansible Automation Platform

In this repo we