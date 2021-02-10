---

copyright:
  years: 2018, 2021
lastupdated: "2021-02-10"

---

# Portieris

This chart installs Portieris in your cluster.

## Prerequisites

* [Kubernetes 1.16](https://v1-16.docs.kubernetes.io/docs/setup/release/notes/) and later
* [Helm 3.0](https://helm.sh/docs/intro/install/) and later

## Chart details

This chart:

* Installs Portieris
* Adds a resource definition for security policies
* Adds some default security policies
* Configures Kubernetes admission webhooks to direct admission requests to Portieris

## Installing the chart

### Regenerate certificates

Checkout the source project at the release level and run the `./gencerts` script.

**Important**: You must run the `./gencerts` script, otherwise the installation uses the default certificates. If you use the default certificates, you will deploy with certificates that are publically accessible on GitHub.

### IBM Cloud Kubernetes Service

If you're deploying onto an IBM Cloud&reg; cluster, Portieris automatically creates policies that allow the various Kubernetes components to be deployed. Portieris also has a policy rule that allows all images to be deployed without any verification. Change `allow everything` because it is not secure, but keep the IBM Cloud specific policies.

```
helm install --create-namespace -n portieris .
```

### Other Kubernetes clusters

If you're deploying onto a generic cluster, Portieris automatically creates a policy that allows all images without verification. This action prevents Portieris from preventing you deploying to your cluster. You must update this policy so that it is more restrictive.

```
helm install --create-namespace -n portieris . --set IBMKubernetesService=false --debug
```

For full installation instructions, see [Installing security enforcement in your cluster](https://cloud.ibm.com/docs/services/Registry?topic=registry-security_enforce#sec_enforcer_install).

## Default security policies

This chart installs default security policies in your cluster. You must modify the default policies or replace them with your own. For more information, see [Default policies](https://cloud.ibm.com/docs/services/Registry?topic=registry-security_enforce#default_policies).

You must apply access control policies to limit who can modify Portieris policies in your cluster. See [Controlling who can customize policies](https://cloud.ibm.com/docs/services/Registry?topic=registry-security_enforce#assign_user_policy).

## Customizing security policies

You can add your own security policies, scoped to a Kubernetes namespace or the entire cluster. Cluster policies are used when no namespace scoped policies exist in the Kubernetes namespace that you are deploying to.

For information about configuring security policies, and an explanation of the security policy resources, see [Customizing policies](https://cloud.ibm.com/docs/services/Registry?topic=registry-security_enforce#customize_policies).

## Removing the chart

Remove the chart.

    ```
    helm delete -n portieris portieris
    ```
