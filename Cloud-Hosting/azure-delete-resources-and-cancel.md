
# Delete Azure resources and cancel a subscription

Run these commands in **Azure Cloud Shell — Bash**. Replace `YOUR-SUBSCRIPTION-ID` before running the deletion script.

**Destructive operation:** this deletes every resource group and its resources, then requests subscription cancellation. Back up anything you need first. Use a Cloud Shell session without mounted storage, or storage hosted in another subscription, so you do not delete the storage supporting your session.

## 1. Find the subscription ID

```bash
az account list --query "[].{Name:name,Subscription:id,State:state}" -o table
```

## 2. Delete resources and request cancellation

```bash
(
set -Eeuo pipefail
trap 'echo "Stopped because a command failed. Subscription cancellation is not confirmed." >&2' ERR

SUB_ID="YOUR-SUBSCRIPTION-ID"

# Require an explicit subscription UUID.
if [[ ! "$SUB_ID" =~ ^[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}$ ]]; then
  echo "Replace YOUR-SUBSCRIPTION-ID with the subscription UUID."
  exit 1
fi

az account set --subscription "$SUB_ID"

echo "TARGET SUBSCRIPTION:"
az account show --subscription "$SUB_ID" \
  --query '{Name:name,Subscription:id,Tenant:tenantId,State:state}' -o table

echo "RESOURCE GROUPS TO DELETE:"
az group list --subscription "$SUB_ID" \
  --query "[].{Name:name,Location:location}" -o table

echo "RESOURCES TO DELETE:"
az resource list --subscription "$SUB_ID" \
  --query "[].{Name:name,Type:type,Group:resourceGroup}" -o table

# Stop on locks; do not silently remove protection.
LOCKS="$(az lock list --subscription "$SUB_ID" -o json)"
if [[ "$(jq 'length' <<< "$LOCKS")" -gt 0 ]]; then
  echo "Deletion locks exist. Review and remove applicable locks, then rerun:"
  jq -r '.[] | [.name, .level, .id] | @tsv' <<< "$LOCKS"
  exit 1
fi

echo
echo "This will delete ALL resource groups and cancel this subscription."
read -r -p "Type the full subscription ID to proceed: " CONFIRM </dev/tty
if [[ "$CONFIRM" != "$SUB_ID" ]]; then
  echo "Aborted. Nothing deleted."
  exit 1
fi

# Capture the list before deleting. Wait for each deletion to finish.
GROUPS_JSON="$(az group list --subscription "$SUB_ID" -o json)"
mapfile -t RG_NAMES < <(jq -r '.[].name' <<< "$GROUPS_JSON")

for RG in "${RG_NAMES[@]}"; do
  echo "Deleting resource group: $RG"
  az group delete \
    --subscription "$SUB_ID" \
    --name "$RG" \
    --yes
done

# Only proceed if Azure reports no remaining groups or listed resources.
RG_COUNT="$(az group list --subscription "$SUB_ID" --query 'length(@)' -o tsv)"
RESOURCE_COUNT="$(az resource list --subscription "$SUB_ID" --query 'length(@)' -o tsv)"

if [[ "$RG_COUNT" != "0" || "$RESOURCE_COUNT" != "0" ]]; then
  echo "Resources or groups remain. Subscription has NOT been canceled."
  az group list --subscription "$SUB_ID" -o table
  az resource list --subscription "$SUB_ID" -o table
  exit 1
fi

echo "Resource checks passed. Requesting subscription cancellation..."
az rest \
  --method post \
  --url "https://management.azure.com/subscriptions/${SUB_ID}/providers/Microsoft.Subscription/cancel?api-version=2021-10-01"

echo "Azure accepted the cancellation. Confirm status in Portal > Subscriptions."
)
```

## Notes and limitations

- If deletion fails, for example because a backup vault has protected items, the script stops before cancellation. Review the error and resolve the blocker before rerunning.
- Cancellation requires the appropriate subscription/billing permissions. If the API rejects the request, cancellation has not been confirmed; use the Azure portal or your billing administrator/provider as applicable.
- This removes active resource groups and their resources. It does not guarantee purging soft-deleted data or canceling separate support plans, Marketplace subscriptions, reservations, or savings-plan commitments. Review these separately under **Cost Management + Billing**.
- These commands have been exported for you to run; they have not been executed against your Azure subscription.

## Microsoft documentation

- [Azure CLI resource-group commands](https://learn.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest)
- [Subscription cancellation REST API](https://learn.microsoft.com/en-us/rest/api/subscription/subscription/cancel?view=rest-subscription-2021-10-01)
- [Cancel and delete your Azure subscription](https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/cancel-azure-subscription)
