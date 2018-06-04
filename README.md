# Falco-extras


This repository contains [falco](https://github.com/draios/falco/wiki) runtime security profiles for a growing list of the most popular container images. If you are using any image from the list below, they will provide you an initial set of security whitelists and constraints that you can extend with your deployment specifics.

## What kind of unwanted behaviors can Falco detect?

Falco can detect and alert on any behavior that involves making Linux system calls. Thanks to Sysdig's core decoding and state tracking functionality, falco alerts can be triggered by the use of specific system calls, their arguments, and by properties of the calling process. For example, you can easily detect things like:

- A shell is run inside a container
- A server process spawns a child process of an unexpected type
- Unexpected read of a sensitive file (like /etc/shadow)
- A non-device file is written to /dev
- A standard system binary (like ls) makes an outbound network connection

## Image list

The current list of ruleset files include:

*   Kubernetes 1.10 cluster components:
    *   apiserver
    *   controller-manager
    *   kube-dns
    *   kube-scheduler
    *   dashboard
*   Google Kubernetes Engine components
*   Apache
*   Consul
*   ElasticSearch
*   etcd
*   Fluentd
*   HAproxy
*   MongoDB
*   Nginx
*   PHP-FPM
*   PostgreSQL
*   Redis
*   Traefik

# How to use these profiles

You can load custom rules for Falco using the [falco_rules.local.yaml](https://github.com/draios/falco/wiki/Falco-Rules#appending-to-lists-rules-and-macros) or the `rules.d` directory.

By default the rules are read in this order:

- /etc/falco/falco_rules.yaml
- /etc/falco/falco_rules.local.yaml
- /etc/falco/rules.d

But you can always change the load order or add adittional files / directories modifying the `falco.yaml` config file and restarting the Falco process.

For example, if you want to include the Traefik rules present in this repository, you can just append the YAML content:

`cat rules/rules-traefik.yaml >> /etc/falco/falco_rules.local.yaml`

If you are deploying Falco as a container itself, then you will mount this file to `/etc/falco/` and relaunch the image.

**Note:** Take into account that these rules are based on strict whitelisting, and they have been created using default images. You need to adapt them to your deployment to avoid false positives. Extend the rules whitelisting the paths you use for caches, user data, extra libraries, specify additional listing ports you processes use, binary utilities etc. You can read more about the [Falco rule syntax here](https://github.com/draios/falco/wiki/Falco-Rules). 
