### 1. If the cluster already exists but Docker was stopped

* Make sure **Docker Desktop** (or Docker Engine on Linux) is running.
* As soon as Docker is up, the `kind-control-plane` container should appear again:
  <pre class="overflow-visible!" data-start="486" data-end="532"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>docker ps --filter </span><span>"name=kind"</span><span>
  </span></span></code></div></div></pre>
* If you see `kind-control-plane` running, your cluster is up — try:
  <pre class="overflow-visible!" data-start="604" data-end="637"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>kubectl get nodes
  </span></span></code></div></div></pre>

### 2. If the cluster was removed or isn’t running

You need to **recreate the cluster** (this is the normal workflow with kind):

<pre class="overflow-visible!" data-start="768" data-end="882"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>kind delete cluster --name kind   </span><span># (safe to run, even if not present)</span><span>
kind create cluster --name kind
</span></span></code></div></div></pre>

This will spin up a new Docker container (`kind-control-plane`) and update your kubeconfig so that `kubectl` connects to it. Then check:

<pre class="overflow-visible!" data-start="1021" data-end="1093"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>kubectl cluster-info --context kind-kind
kubectl get pods -A
</span></span></code></div></div></pre>

### 3. If you just want to “restart” a running cluster

That really means restarting the Docker container:

<pre class="overflow-visible!" data-start="1201" data-end="1246"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"><span class="" data-state="closed"></span></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>docker restart kind-control-plane
</span></span></code></div></div></pre>

(Replace `kind-control-plane` with the actual container name from `docker ps`.)
