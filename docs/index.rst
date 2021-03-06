educates
========

The **educates** project provides a system for hosting interactive workshop environments in Kubernetes. It can be used for self paced or supervised workshops. It can also be useful where you need to package up demos of applications hosted in Kubernetes for users or potential customers.

Users doing a workshop are provided access to a dashboard in their web browser combining a shell environment with the workshop content, including any custom tools required for the workshop. The dashboard can also optionally embed slide content, an IDE, a web console for accessing the Kubernetes cluster (Kubernetes dashboard or Octant), and other custom web applications. Where required for a workshop, deployment of an image registry per workshop session, and the ability to use docker for doing container image builds, can be enabled.

*Note: As yet there has not been an official public first release of this project. We are working through being able to release it as a fully open source project, but this is not yet certain and we don't know when that may occur.*

*Also be aware that the name of the project is in the process of being changed from what was the original name. You will therefore see references to an older name of "eduk8s", as well as interim placeholder name of "edukates". As this process progresses you will need to change to the new name when performing deployments and in creation of workshops.*

*So use at your own extreme risk. It may instantly set fire to anything it touches. If you do attempt to use it, don't assume that things will not be broken by future changes.*


.. toctree::
  :maxdepth: 2
  :caption: Project Details:

  project-details/project-overview
  project-details/sample-screenshots

.. toctree::
  :maxdepth: 2
  :caption: Getting Started:

  getting-started/installing-operator
  getting-started/training-session
  getting-started/under-the-covers
  getting-started/deleting-operator
  getting-started/sample-workshops

.. toctree::
  :maxdepth: 2
  :caption: Installation Guides:

  installation-guides/deploying-to-minikube
  installation-guides/deploying-to-kind

.. toctree::
  :maxdepth: 2
  :caption: Workshop Content:

  workshop-content/workshop-images
  workshop-content/workshop-config
  workshop-content/workshop-instructions
  workshop-content/workshop-runtime
  workshop-content/presenter-slides
  workshop-content/building-an-image
  workshop-content/working-on-content

.. toctree::
  :maxdepth: 2
  :caption: Operator Config:

  runtime-environment/custom-resources
  runtime-environment/workshop-definition
  runtime-environment/workshop-environment
  runtime-environment/workshop-request
  runtime-environment/workshop-session
  runtime-environment/training-portal
  runtime-environment/system-profile

.. toctree::
  :maxdepth: 2
  :caption: Portal REST API:

  portal-rest-api/client-authentication
  portal-rest-api/workshops-catalog
  portal-rest-api/session-management
  portal-rest-api/anonymous-access
