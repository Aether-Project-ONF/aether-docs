.. Repositories
.. ---------------

Aether is assembled from multiple components
spanning several Git repositories. These include repos for different
subsystems (e.g., AMP, SD-Core, SD-RAN), but also for different stages
of the development pipeline (e.g., source code, deployment artifacts,
configuration specs).  This rest of this section identifies all the
Aether-related repositories, with the OnRamp repos listed at the end
serving as the starting point for anyone who wants to come
up to speed on the rest of the system.

.. admonition:: Troubleshooting Hint

  Users are encouraged to join the ``#aether-onramp`` channel of the
  `Aether Workspace <https://aether5g-project.slack.com/>`__ on Slack, where
  questions about using OnRamp to bring up Aether are asked and
  answered. The ``Troubleshooting`` bookmark for that channel includes
  summaries of known issues.

Source Repos
~~~~~~~~~~~~~~~~

Source code for Aether and all of its subsystems can be found in
the following repositories:

* GitHub repository for the Aether Core
  (https://github.com/omec-project): Microservices for SD-Core, plus
  the emulator (gNBsim) that subjects SD-Core to RAN workloads.

* GitHub repository for the ONOS Project
  (https://github.com/onosproject): Microservices for SD-RAN and ROC,
  plus the YANG models used to generate the Aether API.

* GitHub repository for the Aether Project
  (https://github.com/opennetworkinglab): OnRamp documentation and
  playbooks for deploying Aether.

Note that ``omec-project`` and ``opennetworkinglab`` are historical
artifacts related to Aether's origin at the Open Networking Foundation
(ONF).

Artifact Repos
~~~~~~~~~~~~~~~~

Aether includes a *Continuous Integration (CI)* pipeline that builds
deployment artifacts (e.g., Helm Charts, Docker Images) from the
source code. These artifacts are stored in the following registries:

Helm Charts

 | https://charts.aetherproject.org
 | https://charts.onosproject.org
 | https://charts.atomix.io
 | https://sdrancharts.onosproject.org
 | https://charts.rancher.io/

Docker Images

 | https://registry.aetherproject.org
 | https://hub.docker.com

Note that as of version 1.20.8, Kubernetes uses the `Containerd
<https://containerd.io/>`__ runtime system instead of Docker. This is
transparent to anyone using Aether, which manages containers
indirectly through Kubernetes (e.g., using ``kubectl``), but does
impact anyone who directly depends on the Docker toolchain. Also note
that while Aether documentation often refers its use of "Docker
containers," it is now more accurate to say that Aether uses
`OCI-Compliant containers <https://opencontainers.org/>`__.

The Aether CI pipeline keeps these artifact repos in sync with the
source repos listed above. Among those source repos are the source
files for all the Helm Charts:

 | ROC: https://github.com/onosproject/roc-helm-charts
 | SD-RAN: https://github.com/onosproject/sdran-helm-charts
 | SD-Core: https://github.com/omec-project/sdcore-helm-charts

The CI pipeline for each subsystem is implemented as GitHub Actions in
the respective repos. The approach is based on an earlier version
implemented by set of Jenkins jobs, as described in the Lifecycle
Management chapter of a companion Edge Cloud Operations book. Of
particular note, the current pipeline adopts the version control
strategy of the original mechanism.

.. _reading_cicd:
.. admonition:: Further Reading

    L. Peterson, A. Bavier, S. Baker, Z. Williams, and B. Davie. `Edge
    Cloud Operations: A Systems Approach
    <https://ops.systemsapproach.org/lifecycle.html>`__. June 2022.

OnRamp Repos
~~~~~~~~~~~~~~~~~~~

OnRamp adopts minimal Ansible tooling for the *Continuous Deployment
(CD)* half of the CI/CD pipeline. The approach is designed to make it
easy to take ownership of the configuration parameters that define
your specific deployment scenario.  The rest of this guide walks you
through a step-by-step process of deploying and operating Aether on
your own hardware.  For now, we simply point you at the collection of
OnRamp repos:

 | Deploy Aether: https://github.com/opennetworkinglab/aether-onramp
 | Deploy 5G Core: https://github.com/opennetworkinglab/aether-5gc
 | Deploy 4G Core: https://github.com/opennetworkinglab/aether-4gc
 | Deploy Management Plane: https://github.com/opennetworkinglab/aether-amp
 | Deploy SD-RAN: https://github.com/opennetworkinglab/aether-sdran
 | Deploy OAI Software Radio: https://github.com/opennetworkinglab/aether-oai
 | Deploy srsRAN Software Radio: https://github.com/opennetworkinglab/aether-srsran
 | Deploy gNB Simulator: https://github.com/opennetworkinglab/aether-gnbsim
 | Deploy UE+gNB Simulator: https://github.com/opennetworkinglab/aether-ueransim
 | Deploy Kubernetes: https://github.com/opennetworkinglab/aether-k8s

It is the first repo that defines a way to integrate all of the Aether
artifacts into an operational system. That repo, in turn, includes the
other repos as submodules. Note that each of the submodules is
self-contained if you are interested in deploying just that subsystem,
but this guide approaches the deployment challenge from an
integrated, end-to-end perspective.

There are two other Aether-related repos of note; they are **not**
managed as submodules of ``aether-onramp``:

 | Aether Documentation: https://github.com/opennetworkinglab/aether-docs
 | Jenkins Pipelines: https://github.com/opennetworkinglab/aether-jenkins

.. admonition:: Troubleshooting Hint

  The ``master`` branch of the OnRamp repo corresponds to the latest
  *minimally-validated* configuration of Aether, which is known to
  work for a subset of the available blueprints (always including
  *Quick Start*), but may not run consistently for *all* blueprints.
  Once all integration tests (Jenkins Pipelines) consistently succeed
  for a period of time (~2 weeks) a tagged version of OnRamp is
  created. If you are having trouble with ``master`` working for a
  particular blueprint, you should try the latest tagged version:
  https://github.com/opennetworkinglab/aether-onramp/tags.

Finally, because OnRamp uses Ansible as its primary deployment tool, a
general understanding of Ansible is helpful (see the suggested
reference).  However, this guide walks you, step-by-step, through the
process of deploying and operating Aether, so previous experience with
Ansible is not a requirement. Note that Ansible has evolved to be both
a "Community Toolset" anyone can use to manage a software deployment,
and an "Automation Platform" offered as a service by RedHat. OnRamp
uses the toolset, but not the platform/service.

.. _reading_ansible:
.. admonition:: Further Reading

   `Overview: How Ansible Works <https://www.ansible.com/overview/how-ansible-works>`__.

