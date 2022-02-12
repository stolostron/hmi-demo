# hmi-demo

Description: This is a demo of a customer deployment of Inductive Automation Ignition application to RHEL Edge device including some of the external services required by the customer for security, logging and certification so the application can run in HTTPS mode in a completely disconnected environment.

![architecture_image](images/arch.gif)

## Steps to install

1. Install OpenShift Gitops so we can use it to deploy Ansible Automation Platform.

    ```shell
    oc apply -k https://github.com/redhat-cop/gitops-catalog/openshift-gitops-operator/overlays/stable
    ```

2. Create service account so we can install Ansible Automation Platform via Openshift Gitops.

    ```shell
    oc create -f argo/argocd-clusterbindingrole.yaml
    ```

3. Create Openshift Gitops application to install Ansible Automation Platform Operator.

    ```shell
    oc create -f argo/operator.yaml
    ```

4. Create OpenShift Gitops application to install an instance of Ansible Automation Platform Controller.

    ```shell
    oc create -f argo/controller.yaml
    ```

5. Log in to controller in a web browser via the address in oc get route in the HOST/PORT section

    ```shell
    oc get route -n ansible-automation-platform
    NAME       HOST/PORT                                                                      PATH   SERVICES           PORT   TERMINATION     WILDCARD
    hmi-demo   hmi-demo-ansible-automation-platform.apps.brooklyn.demo.red-chesterfield.com          hmi-demo-service   http   edge/Redirect   None
    ```

6. Login with credential username Admin and the password from below command:

    ```shell
    oc get secrets/hmi-demo-admin-password -n ansible-automation-platform -o yaml
    ```

    NOTE: The password you want is the one in the stringData password field that looks like this: ```shell "stringData":{"password":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXX"}```

7. Once logged in you will be prompted for your console.redhat.com credentials to choose which subscription to use.

## Configuring Ansible Automation Platform
