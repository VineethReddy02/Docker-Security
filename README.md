# Docker-Security

- Kernel Namespaces are created when a kernel is run.
- Containers processes cannot see or affect other containers.
- Each cntainer has its own network stack.

- The second thing created when we run a container is Control Groups(cgroups)
- cgroups provide the resource accounting, limiting it ensures that the no container exhausts the         host resources. Even if a attacker gains access to a container he cannot attack the other 100's   of   containers on the host as the cgroups prevent it.

### Security of Docker Daemon

- Only trusted users must get the control over docker daemon as it runs on the host with root             previleges.
- Docker runs the containers with restricted capabilities. For example, even OS containers don't get     full root privilages. 
- Docker Daemon is confined to a host but we need to make sure entire docker cluster is secure i.e both   managers and workers in docker swarm.

### Mutually Authenticated TLS (MTLS)

- Manually rotating the certificates is painful amn all the nodes that make a docker swarm cluster.
- Rotating security certificates without downtime is harder.
- MTLS is built into docker swarm and solves these challenges. This rotates certificates between the     hosts or nodes at a set interval of time.

Initiating Key Rotation:

If you are concerned that a swarm node is compromised, you can easily run 
```docker swarm ca --rotate``` to generate a new CA certificate and key. This will be automatically   distributed to all managers and workers.

Swarm Managers

- Maintain the cluster state.
- Schedule services.
- Serve as Swarm HTTP API endpoints.

Swarm Workers

- Sole purpose is to execute container workloads.

### Securing Docker to Registry Communication
DTR

### Docker Content Trust

- It's important to know that the image you wanted is the image you received, especially when pulling   images from internet sites or in large organisations.
- Docker Notary is an open source project that provides the ability to digitally sign content and is   also included with DTR.
- Enforcs a policy that any operations with a remote registry require signed images. SO any image you   looking to pull or push needs to be signed.
- To enable docker content trust.
  Enabled with ```export DOCKER_CONTENT_TRUST=1```
- Once enabled, the docker push will try to sign an image.
- Once enabled, a docker pull will only download signed images.
- With UCP, you can ensure that only signed images can be run.

### Docker Access Control Model

1. Subjects
   User, Team, Organization, Service account.
   Subjects are granted roles.
   
2. Roles
   A role defines what can be done.
   - View only.
   - Restricted control.
   
3. Resources
   This are resource sets.
   - Swarm collections
   - Kubernetes namespaces.
   Collections and namespaces together are called resource sets.

4. Grants
   - A grant is a combination of a subject, role and resource set.
   - Grants are the Access Control List(ACL) of docker UCP.
   
Docker trusted registry provides the image scanning and lists the CVE's.
