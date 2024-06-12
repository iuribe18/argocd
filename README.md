# GitOps with ArgoCD - KodeKloud

# What is GitOps?
GitOps is a framework where the entire code delivery process is controlled via Git. It also includes infrastructure and application definition as code and automation to deploy changes and rollbacks. GitOps can be considered as an extension of infrastructure code that uses Git as a version control system.
Developers commit to a shared repository using a version control system such as Git. Usually, a feature branch is created, which is a copy of the main code base wherein a team of developers can work on a new feature until it is completed. A continuous integration service automatically builds and run unit tests on the new code and merges the code into a central repository.
Before merging, the changes are reviewed and approved by the concerned team. A continuous deployment is the final stage in the pipeline that refers to the automatic releasing of any developer changes from the repository to the clusters. The core idea here is to have the infrastructure, applications and all related components declaratively defined in one or more Git repositories.
In the GitOps scenario, and automated process ensures that the desired state in the repository and the actual state in the production environment always matches. A GitOps operator runs within one of the kubernetes cluster. It continuously monitors and pulls changes from Git repository and applies them within the cluster it is running.

# GitOps Principles
GitOps has 4 principles.
1. The first principle is about the declarative versus imperative approach. GitOps demands the entire system, including infrastructure and application manifest to be declared in a declarative state.
2. The second principle is to make use of Git. All the declarative files, also known as the desired state, is stored in a Git repository. It provides a version control and also enforces immutability. Since the desired state is stored and versioned, it is essentially known as the source of truth state. Once we store the desire state in Git, we must allow any changes to the state to be applied automatically. This brings us to the third principle. 
3. GitOps operators, also known as software agents, automatically pull the desired state from Git and apply them in one or more environments or clusters.
4. The final principle talks about reconciliation. The GitOps operator also make sure that the entire system is self-healing to reduce the risk of human errors. The operator continuously loops through three steps, observe, diff and act.

# GitOps Pipeline
Within a GitOps pipeline, we essentially have two Git repositories, one for the application code, and the other for kubernetes manifest. The continuous integration steps remains the same till publishing an image to the container registry. The next step is to clone configuration repository and, for example, update the manifest with the new image name, commit and push to the branch.
Finally, the pipeline with also raise a pull request to the manifest repository. A team member will review the pull request, suggests any changes, or if everything is fine, approves it and merges the changes. At this point, the Git operator, running within a kubernetes cluster, will pull the changes from the repository and synchronizes them within the cluster.
To summarize, in the DevOps pipeline, the changes were pushed to the cluster, and in GitOps, the changes were pulled by the operator.

# Concepts
Application: A group of kubernetes resources as defined by a manifest.
Application source type: The tool is used to build the application, sucas Helm, Kustomize or Ksonnet. 
Project: Provide a logical grouping of applications, which is useful when argoCD is used by multiple teams.

Target State: The desire state of an application, as represented by files in a Git repository.
Live state: The live state of that application. What pods, configmaps, secrets, etc are created/deployed in a kubernetes cluster.

Sync status: Whether or not the live state matches the target state. Is the deployed application the same as Git says it should be?
Sync: The process of making an application move to its target state. E.g. by applying changes to a kubernetes cluster.
Sync operation status: Whether or not a sync succeeded.

Refresh: Compare the latest code in Git with the live state. Figure out what is different.
Health: The health of the application, is it running correctly? Can it serve requests?