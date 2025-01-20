# Deploy Newt Wireguard client to your Kubernetes environment

## About Newt
Newt is a client created for working together with Pangolin. This is useful to connect your own infrastructure to outside without the need of third party services like Cloudflare tunnels. This helm chart is created to allow you to quicklly add a Kubernetes workload to the Internet without needing to open up your own infrastructure. 

more: https://docs.fossorial.io/Newt/overview

## About Pangolin
Pangolin is a self-hosted reverse proxy solution that utilizes WireGuard to create secure tunnels. This permits users to access their private applications and services remotely without needing to reconfigure their router for port forwarding—an often cumbersome and risky process. The service is especially beneficial for those who prioritize privacy and security, as it allows for better control over their setups compared to cloud-based alternatives.

more: https://docs.fossorial.io/Pangolin/overview

## Install

Before installing ensure you have installed the Pangolin server side or have an existing service that you can configure a Site on.
If not check the documentation of Pangolin to get started quickly. I am using a small Hetzner cloud VM for it and a cloud-init script. 

In that environment create a new Site and retrieve these variables:
- id
- secret
- endpoint

Clone the repository and install the helm chart on your Kubernetes with these commands:

```bash
helm install newt -n newt --create-namespace /
--set newt.id=XYZ /
--set newt.secret=XYZ /
--set newt.endpoint=https://xyz
.
```

## Configuration

All configuration in done in Pangolin so apart from the install values nothing is needed.
Availability monitoring is visible there as well, so any custom metrics and logging is not part of this helm chart.

### Unlocking Kubernetes workloads

In Pangolin you can add the workload by either entering the ingress host name if you want all traffic to go via ingress.

But easiest it is to use the servicename and namespace. So a service with the name website in the namespace wordpress running on port 8080 you can add as http://website.wordpress:8080

## Security

Standard the namespace isolation ensures that workloads are secured between them except published services.
If you want to increase security you can explicitly allow the newt containers to only reach out to whitelisted services using NetworkPolicies. This removes some of the security but also ensures that services that you do not want to expose like databases are not available public.

In examples a NetworkSecurityPolicy (NSP)i s added allowing only access from newt to pods that have the annotation `newt:enabled`.
More about NSP can be found on https://sfasdfasdfa
