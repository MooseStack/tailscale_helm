# tailscale_helm

Deploy Tailscale as a Subnet Router(VPN like/forwarder from a specific pod) using Helm to OpenShift/Kubernetes.

## Prerequisite:
1. `git clone https://github.com/mdbell47/tailscale_helm.git && cd tailscale_helm`

2. Add tag in Tailscales ACL in [access controls](https://login.tailscale.com/admin/acls/file) tab
 ```
 	"tagOwners": {
		"tag:tailscale": ["autogroup:admin"],
	},
 ```

3. Generate an oAuth client token from [Tailscale's Settings tab](https://login.tailscale.com/admin/settings/oauth)
   - `Description` : Optional description
   - Permissions:
     - `Devices` -> `Core` -> `Read and write` -> attach `tailscale` tag.
     - `Keys` -> `Auth Keys` -> `Read and write` -> attach `tailscale` tag.
   - Copy the `Client secret` for next step.
  
4. Update [values.yaml](tailscale/values.yaml) `secret` section:
5. - `TS_AUTHKEY` : paste the Client Secret from previous step.
   - `TS_ROUTES` : the subnet block that you want to access, can be comma separated.
   - `TS_EXTRA_ARGS` : you can leave this as-is. Currently, using tailscale as a subnet router. So those extra args are required for it to work.
  
## Deploy Tailscale
`helm install -f tailscale/values.yaml tailscale --namespace tailscale --name-template tailscale`

## After Deployment
1. Go back to [Tailscale's Machines tab](https://login.tailscale.com/admin/machines)

2. Wait one minute for the machine/pod to connect. Select the options/actions and verify these are already done (should be automated due to ACL/permissions we gave):
   - `Disable key expirey` if needed.
   - `Edit route settings` -> check the box for the `subnet routes` and `Use as exit node` -> Save

3. You can now install the Tailscale mobile app/desktop/VsCode extention/etc, login, and choose the pod as Exit Node to gain VPN/forwarder-like access. Enjoy!