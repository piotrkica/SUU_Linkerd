## Step 0: Setup

Before anything else, we need to ensure you have access to modern Kubernetes cluster and a functioning kubectl command on your local machine. (If you don’t already have a Kubernetes cluster, one easy option is to run one on your local machine. There are many ways to do this, including kind, k3d, Docker for Desktop, and more.)

Validate your Kubernetes setup by running:

```bash
kubectl version --short
```

You should see output with both a Client Version and Server Version component.

Now that we have our cluster, we’ll install the Linkerd CLI and use it validate that your cluster is capable of hosting Linkerd.

## Step 1: Install the CLI

If this is your first time running Linkerd, you will need to download the linkerd CLI onto your local machine. The CLI will allow you to interact with your Linkerd deployment.

To install the CLI manually, run:

```bash
curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/install | sh
```

Be sure to follow the instructions to add it to your path.

Once installed, verify the CLI is running correctly with:

```bash
linkerd version
```

You should see the CLI version, and also Server version: unavailable. This is because you haven’t installed the control plane on your cluster. Don’t worry—we’ll fix that soon enough.

## Step 2: Validate your Kubernetes cluster

Kubernetes clusters can be configured in many different ways. Before we can install the Linkerd control plane, we need to check and validate that everything is configured correctly. To check that your cluster is ready to install Linkerd, run:

```bash
linkerd check --pre
```

If there are any checks that do not pass, make sure to follow the provided links and fix those issues before proceeding.

## Step 3: Install Linkerd onto your cluster

Now that you have the CLI running locally and a cluster that is ready to go, it’s time to install Linkerd on your Kubernetes cluster. To do this, run:

```bash
linkerd install --crds | kubectl apply -f -
```

followed by:

```bash
linkerd install | kubectl apply -f -
```

These commands generate Kubernetes manifests with all the core resources required for Linkerd (feel free to inspect this output if you’re curious). Piping these manifests into kubectl apply then instructs Kubernetes to add those resources to your cluster. The install --crds command installs Linkerd’s Custom Resource Definitions (CRDs), which must be installed first, while the install command installs the Linkerd control plane.

Depending on the speed of your cluster’s Internet connection, it may take a minute or two for the control plane to finish installing. Wait for the control plane to be ready (and verify your installation) by running:

_linkerd check_

[@linkerd:tutorial]
