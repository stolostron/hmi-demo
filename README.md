# hmi-demo

Description: This is a demo of a customer deployment of Inductive Automation Ignition application to RHEL Edge device including some of the external services required by the customer for security, logging and certification so the application can run in https mode in a completely disconnected environment.

![architecture_image](images/arch.gif)

## Steps to install

1. Install Ansible Automation platform using the operator with OpenShift Gitops

```shell
oc apply -k https://github.com/redhat-cop/gitops-catalog/ansible-automation-platform/instance/overlays/default -n ansible-automation-platform && oc apply -k https://github.com/redhat-cop/gitops-catalog/ansible-automation-platform/instance/overlays/default
```
