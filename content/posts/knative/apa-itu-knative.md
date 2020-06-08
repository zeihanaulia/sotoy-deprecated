+++
title = "Apa itu Knative"
categories = [
    "belajar",
    "knative",
]
tags = [
    "knative",
]
date = "2020-05-10"
+++

Akhir akhir ini serverless sedang booming. Beberapa penyedia layanan cloud computing sudah menyediakan product serverless.
Misalnya pada AWS, ada AWS Lambda dimana kita sebagai pengembang tinggal upload binary function ke AWS lambda.
Lalu pada GCP ada Cloud Function mirip seperti AWS Lambda dan belum lama ini mulai ramai Cloud Run yang mana sebenernya Cloud Run itu adalah [Knative](https://twitter.com/ahmetb/status/1195056373983145984).

Knative sendiri berjalan diatas kubernetes atau extention dari kubernetes yang menjadi middleware untuk membuat aplikasi yang bisa berjalan dimanapun pada cloud yang menggunakan kubernetes. Aplikasinya harus berbasis yang kontainer ya.

Fungsi dari Knative ada 2 yaitu:

- run stateless workloads such as microservices
- event subscription, delivery and handling

Knative fokus ke dua komponen kunci, yaitu:, melayani trafffic (Knative Serving) dan memungkinkan aplikasi dengan mudah mengkonsumsi dan memproduksi events (Knative Eventing). Sebelumnya ada 3 komponen yaitu [Knative Build](https://github.com/knative/build). Komponen ini sudah tidak digunakan lagi atau deprecated bisa dilihat diissue [#614](https://github.com/knative/build/issues/614). Alasannya karena sudah ada beberapa tools lain yang lebih fokus ke build contohnya [Tekton Pipeline](https://github.com/tektoncd/pipeline) atau [Kaniko](https://github.com/GoogleContainerTools/kaniko) atau [buildpacks](https://github.com/buildpacks) atau mau yang lebih kompleks bisa pake Jenkins dll.

1. [Knative Serving](https://github.com/knative/serving)
    Bagaimana kode kita menerima requests dan scaling dengan mereka (semua request itu).

    Detail dari Knative serving:

       - Request-driven compute runtime
       - Scale-to-zero / scale out per load
       - Deploy from container registry
       - Multiple revision of same app
       - Route traffic across revisions

2. [Knative Eventing](https://github.com/knative/eventing)

    Bagaimana kode kita ditrigger oleh events

    Knative Eventing details:

        - Apps and functions consume and publish event streams
        - Multiple event source available
        - Encourages asynchronouse, loosely coupled architechture.

## Install Knative

Knative punya 2 komponen yang diinstall secara independen dan membutuhkan kubernetes v.1.5 atau yang terbaru.
Dan tulisan ini mengasumsikan kita sudah menginstall kubenetes cluster dikomputer.

### Knative Serving

Tulisan ini diambil dari [https://knative.dev/docs/install/any-kubernetes-cluster/](https://knative.dev/docs/install/any-kubernetes-cluster/).

#### 1. Install Curtom Resource Definitions (aka CRDs)

```bash
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.14.0/serving-crds.yaml
```

#### 2. Install core komponen dari serving

```bash
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.14.0/serving-core.yaml
```

#### 3. Install Istio untuk Knative

Pada tulisan ini kita menggukan isto sebagai networking layer, Meski ada beberapa pilihan lain. misalnya:

- [Ambassador](https://github.com/datawire/ambassador)
- [Contour](https://github.com/projectcontour/contour)
- [Gloo](https://github.com/solo-io/gloo)
- [Kourier](https://github.com/knative/net-kourier)

Mungkin ditulisan lain akan mereview ke 5 pilihan networking layer.

##### Download istion dan install

1. Download istio
    ```bash
    # Download and unpack Istio
    export ISTIO_VERSION=1.4.6
    curl -L https://git.io/getLatestIstio | sh -
    cd istio-${ISTIO_VERSION}
    ```
2. Install istio CRDs

    ```bash
    for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done
    ```    

3. Bikin istio namespace.

    ```bash
    cat <<EOF | kubectl apply -f -
    apiVersion: v1
    kind: Namespace
    metadata:
    name: istio-system
    labels:
        istio-injection: disabled
    EOF
    ```
4. Instal istio tanpa sidecar injection

    Dalam menggunakan istio pada Knative kita bisa memilih apakah mau menggunakan [automatic `sidecar injection`](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) atau yang [manual](https://istio.io/docs/setup/additional-setup/sidecar-injection/#manual-sidecar-injection).

    Dan pada tulisan kali ini kami memilih untuk yang manual, alsannya  kita tidak membutuhkan service mesh pada tulisan ini. hanya butuh traffic routing dan ingress.

    Kita install menggunakan [helm](https://helm.sh/docs/intro/install/)

    ```bash
    helm template --namespace=istio-system \
    --set prometheus.enabled=false \
    --set mixer.enabled=false \
    --set mixer.policy.enabled=false \
    --set mixer.telemetry.enabled=false \
    `# Pilot doesn't need a sidecar.` \
    --set pilot.sidecar=false \
    --set pilot.resources.requests.memory=128Mi \
    `# Disable galley (and things requiring galley).` \
    --set galley.enabled=false \
    --set global.useMCP=false \
    `# Disable security / policy.` \
    --set security.enabled=false \
    --set global.disablePolicyChecks=true \
    `# Disable sidecar injection.` \
    --set sidecarInjectorWebhook.enabled=false \
    --set global.proxy.autoInject=disabled \
    --set global.omitSidecarInjectorConfigMap=true \
    --set gateways.istio-ingressgateway.autoscaleMin=1 \
    --set gateways.istio-ingressgateway.autoscaleMax=2 \
    `# Set pilot trace sampling to 100%` \
    --set pilot.traceSampling=100 \
    --set global.mtls.auto=false \
    install/kubernetes/helm/istio \
    > ./istio-lean.yaml

    kubectl apply -f istio-lean.yaml
    ```

#### install knative istio controller

```bash
kubectl apply --filename https://github.com/knative/net-istio/releases/download/v0.14.0/release.yaml
```

#### Fetch the External IP or CNAME:

```bash
kubectl --namespace istio-system get service istio-ingressgateway
```

#### Varifikasi instalasi istio

pastikan istio sudah berjalan dengan mengecek podsnya

```bash
kubectl get pods --namespace istio-system
```

#### Seting domain local pada etc/host

```bash
127.0.0.1 helloworld-go.default.knative.local
```

Kita coba untuk menjalankan [hello-world project](https://knative.dev/docs/serving/samples/hello-world/helloworld-go/).

### Jalankan di browser

```bash
export DOMAIN_NAME=`kubectl get route helloworld-go --output jsonpath="{.status.url}" | cut -d'/' -f 3`
echo -e "127.0.0.1 $DOMAIN_NAME" | sudo tee -a /etc/hosts  
```

Referensi:

- [knative.dev/docs](https://knative.dev/docs/)
- [Getting Started with Knative](https://learning.oreilly.com/library/view/getting-started-with/9781492047025) by [@bryanfriedman](https://twitter.com/bryanfriedman), [@BrianMMcClain](https://twitter.com/BrianMMcClain)
- [Developing serverless applications on Kubernetes with Knative (sponsored by Pivotal) - Bryan Friedman (Pivotal), Brian McClain (Pivotal)](https://learning.oreilly.com/videos/oscon-2019/9781492050643/9781492050643-video325891)
- [Knative = Kubernetes Networking++](https://ahmet.im/blog/knative-better-kubernetes-networking/)