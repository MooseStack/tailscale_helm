# tailscale_helm

Deploy Tailscale as a Subnet Router(VPN like/forwarder from a specific pod) using Helm to OpenShift/Kubernetes.

## Prerequisite:
1. Generate an auth key from [Tailscale's Machines tab](https://login.tailscale.com/admin/machines/new-linux)
   - Enable Tags
   - Enable Ephemeral (to remove stale connections when pod restarts)
   - Enable "Use as exit node"
   - Enable "Reusable"

2. Update [values.yaml](tailscale/values.yaml) `secret` section:
   - `TS_ROUTES` : the subnet block that you want to access, can be comma separated.
   - `TS_AUTHKEY` : the auth key from step #1
   - `TS_EXTRA_ARGS` : you can leave this as-is. Currently, using tailscale as a subnet router. So those extra args are required to work.
  
## Deploy Tailscale
`helm install -f tailscale/values.yaml tailscale --namespace tailscale --name-template tailscale`

## After Deployment
1. Go back to [Tailscale's Machines tab](https://login.tailscale.com/admin/machines)

2. Wait one minute for the machine/pod to connect. Select the options/actions:
   - `Disable key expirey` if needed.
   - `Edit route settings` -> check the box for the `subnet routes` and `Use as exit node` -> Save

3. You can now install the Tailscale mobile app/desktop/VsCode extention/etc, login, and choose the pod as Exit Node to gain VPN/forwarder-like access. Enjoy!