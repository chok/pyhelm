======
PyHelm
======

Python bindings for the Helm package manager

How to use PyHelm
-----------------
In order to install a Helm chart using PyHelm, you can perform the following steps:

**Loading a chart using ChartBuilder**

.. code-block:: python

    from pyhelm.chartbuilder import ChartBuilder

    chart = ChartBuilder({"name": "nginx-ingress", "source": {"type": "repo", "location": "https://kubernetes-charts.storage.googleapis.com"}})

This will cause the chart to be cloned locally, and any additional use of ``chart`` will reference the local copy.
You can also use a local chart by using ``"type": "directory"``, as well as cloning from a git repo using ``"type": "git"``

**Installing a chart**

.. code-block:: python

    from pyhelm.chartbuilder import ChartBuilder
    from pyhelm.tiller import Tiller

    tiller = Tiller(TILLER_HOST)
    chart = ChartBuilder({"name": "nginx-ingress", "source": {"type": "repo", "location": "https://kubernetes-charts.storage.googleapis.com"}})
    tiller.install_release(chart.get_helm_chart(), dry_run=False, namespace='default')

This snippet will install the ``nginx-ingress`` chart on a Kubernetes cluster where Tiller is installed (assuming ``TILLER_HOST`` points to a live Tiller instance). Take note that in most Helm installations Tiller isn't accessible in such a manner, and you will need to perform a Kubernetes port-forward operation to access Tiller.
The ``Tiller`` class supports operations other than installation, including release listing, release updating, release uninstallation and getting release contents.

Helm gRPC
---------
The helm gRPC libraries are located in the hapi directory.  They were generated with the grpc_tools.protoc utility against Helm 2.11.  Should you wish to re-generate them you can easily do so:

.. code-block:: shell

    git clone https://github.com/kubernetes/helm ./helm
    python -m grpc_tools.protoc -I helm/_proto --python_out=. --grpc_python_out=. _proto/hapi/chart/*
    python -m grpc_tools.protoc -I helm/_proto --python_out=. --grpc_python_out=. _proto/hapi/services/*
    python -m grpc_tools.protoc -I helm/_proto --python_out=. --grpc_python_out=. _proto/hapi/release/*
    python -m grpc_tools.protoc -I helm/_proto --python_out=. --grpc_python_out=. _proto/hapi/version/*
