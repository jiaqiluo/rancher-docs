---
title: Overview
---

<head>
  <link rel="canonical" href="https://ranchermanager.docs.rancher.com/integrations-in-rancher/fleet/overview"/>
</head>

Continuous Delivery with Fleet is GitOps at scale. Fleet is designed to manage up to a million clusters. It’s also lightweight enough that it works great for a [single cluster](https://fleet.rancher.io/installation#default-install) too, but it really shines when you get to a [large scale](https://fleet.rancher.io/installation#configuration-for-multi-cluster). By large scale we mean either a lot of clusters, a lot of deployments, or a lot of teams in a single organization.

Fleet is a separate project from Rancher, and can be installed on any Kubernetes cluster with Helm.


## Architecture

For information about how Fleet works, see the [Architecture](./architecture.md) page.

## Accessing Fleet in the Rancher UI

Fleet comes preinstalled in Rancher and is managed by the **Continuous Delivery** option in the Rancher UI. For additional information on Continuous Delivery and other Fleet troubleshooting tips, refer [here](https://fleet.rancher.io/troubleshooting).

Users can leverage continuous delivery to deploy their applications to the Kubernetes clusters in the git repository without any manual operation by following **gitops** practice.

Follow the steps below to access Continuous Delivery in the Rancher UI:

1. Click **☰ > Continuous Delivery**.

1. Select your namespace at the top of the menu, noting the following:

    - By default, **fleet-default** is selected which includes all downstream clusters that are registered through Rancher.

    - You may switch to **fleet-local**, which only contains the **local** cluster, or you may create your own workspace to which you may assign and move clusters.

    - You can then manage clusters by clicking on **Clusters** on the left navigation bar.

1. Click on **Gitrepos** on the left navigation bar to deploy the gitrepo into your clusters in the current workspace.

1. Select your [git repository](https://fleet.rancher.io/gitrepo-add) and [target clusters/cluster group](https://fleet.rancher.io/gitrepo-targets). You can also create the cluster group in the UI by clicking on **Cluster Groups** from the left navigation bar.

1. Once the gitrepo is deployed, you can monitor the application through the Rancher UI.

## Windows Support

For details on support for clusters with Windows nodes, see the [Windows Support](./windows-support.md) page.

## GitHub Repository

The Fleet Helm charts are available [here](https://github.com/rancher/fleet/releases).

## Using Fleet Behind a Proxy

For details on using Fleet behind a proxy, see the [Using Fleet Behind a Proxy](./use-fleet-behind-a-proxy.md) page.

## Helm Chart Dependencies

In order for Helm charts with dependencies to deploy successfully, you must run a manual command (as listed below), as it is up to the user to fulfill the dependency list. If you do not do this and proceed to clone your repository and run `helm install`, your installation will fail because the dependencies will be missing.

The Helm chart in the git repository must include its dependencies in the charts subdirectory. You must either manually run `helm dependencies update $chart` or run `helm dependencies build $chart` locally, then commit the complete charts directory to your git repository. Note that you will update your commands with the applicable parameters

## Troubleshooting

- **Known Issue**: clientSecretName and helmSecretName secrets for Fleet gitrepos are not included in the backup nor restore created by the [backup-restore-operator](../../how-to-guides/new-user-guides/backup-restore-and-disaster-recovery/back-up-rancher.md#1-install-the-rancher-backup-operator). We will update the community once a permanent solution is in place.

- **Temporary Workaround**: By default, user-defined secrets are not backed up in Fleet. It is necessary to recreate secrets if performing a disaster recovery restore or migration of Rancher into a fresh cluster. To modify resourceSet to include extra resources you want to backup, refer to docs [here](https://github.com/rancher/backup-restore-operator#user-flow).

-  **Debug logging**: To enable debug logging of Fleet components, create a new **fleet** entry in the existing **rancher-config** ConfigMap in the **cattle-system** namespace with the value `{"debug": 1, "debugLevel": 1}`. The Fleet application restarts after you save the ConfigMap.

## Documentation

See the [official Fleet documentation](https://fleet.rancher.io/) to learn more.
