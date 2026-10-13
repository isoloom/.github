<p align="center">
  <a href="https://www.isoloom.com"><img src="isoloom-mark.svg" width="84" alt="Isoloom"></a>
</p>

<h1 align="center">Isoloom</h1>

<p align="center">
  <strong>One spec. Every environment.</strong><br>
  Describe your machines, networks and services once. Run them on Docker, Kubernetes, local VMs, Proxmox or six clouds, with the same behavior everywhere, proven by the same checks.
</p>

<p align="center">
  <a href="https://www.isoloom.com">Website</a> ·
  <a href="https://www.isoloom.com/en/docs/introduction">Docs</a> ·
  <a href="https://www.isoloom.com/en/docs/quick-start">Quick start</a> ·
  <a href="https://www.isoloom.com/en/docs/spec-reference">Spec reference</a>
</p>

---

```yaml
# isoloom.yml
version: 1
name: invoice-portal
networks:
  app: { cidr: 10.20.0.0/24 }
machines:
  database:
    networks: { app: 32 }
    services: [{ port: 5432 }]
    docker: { image: "postgres:16" }                     # as a container
    vm: { os: debian-12, provision: [provision/db.sh] }  # as a VM, services installed natively
  web:
    networks: { app: 31 }
    services: [{ port: 8080, http: true }]
    depends_on: [database]
    docker: { build: build/web }
    vm: { os: debian-12, provision: [provision/web.sh] }
checks: [checks/portal-answers.sh]
```

```text
$ isoloom targets
✓ docker  ✓ hosted  ✓ docker-vm  ✓ cloud-docker  ✓ kubernetes  ✓ vagrant  ✓ proxmox  ✓ cloud-vm
$ isoloom run docker && isoloom test docker
```

**Behavior first.** A spec describes what an environment looks like from outside. Each machine
can be produced as a container, a VM, or both, and the targets it can run on are derived from
that. Black-box checks run on every target to prove the behavior is the same.

| Repository | What it is |
| --- | --- |
| [isoloom](https://github.com/isoloom/isoloom) | The `isoloom` command line and the `isoloom-core` library, in Rust |
| [website](https://github.com/isoloom/website) | [www.isoloom.com](https://www.isoloom.com) and its documentation |
| [goad](https://github.com/isoloom/goad) | Game of Active Directory through Isoloom |

Isoloom is at **v0.10**: generators for Docker Compose, Kubernetes, Vagrant (VirtualBox, VMware,
Parallels, libvirt, UTM, QEMU), Docker on one VM, Proxmox, one cloud VM per machine and Docker on a
cloud VM (AWS, Azure, Google Cloud, DigitalOcean, Linode, Oracle), hybrid, machines that already
exist, and cloud services from your own Terraform module. `isoloom run`, `test`, `status`,
`connect`, `exec`, `capture`, `reset` and `down` drive a running environment; Windows machines,
Ansible from a controller, link impairment, instances and a GitHub Action. Release binaries for
macOS, Linux and Windows. See the [roadmap](https://www.isoloom.com/en/docs/roadmap).
