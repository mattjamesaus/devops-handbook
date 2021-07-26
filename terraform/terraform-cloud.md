## Workspaces -> Github Repo
```
curl \                          
--header "Authorization: Bearer $TF_API_TOKEN" \
--header "Content-Type: application/vnd.api+json" \
https://app.terraform.io/api/v2/organizations/growtix/workspaces | jq '.data[] | .attributes | {"vcs-repo", "name"}'
```
