# ASI Cloud

Plugin that connects agents to [ASI Cloud](https://asi.co.nz/cloud/) through ASI Cloud's official remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

ASI Cloud runs virtual machines, S3 object storage and AI Inference in New Zealand data centres. Connect your ASI Cloud account to see and change your services, check your bill and open support tickets by asking.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **ASI Cloud**.
3. Click **Install**, then complete the ASI Cloud sign-in prompt.

Or run `/add-plugin asi-cloud` in chat. In Grok Bot (0.49 or newer), connect **ASI Cloud** from the connector list.

## MCP

```json
{
  "mcpServers": {
    "asi-cloud": {
      "type": "http",
      "url": "https://cloud.asi.nz/mcp"
    }
  }
}
```

Auth is OAuth 2.1 with PKCE and dynamic client registration. An unauthenticated request returns `401` with `resource_metadata` pointing at `https://cloud.asi.nz/.well-known/oauth-protected-resource/mcp`, and the authorization server is `https://cloud.asi.nz`. The client prompts for ASI Cloud sign-in when the plugin connects. There is no client ID to configure.

On the consent screen you choose which of your ASI Cloud organisations the connection can manage. It can do what you can do in the ASI Cloud portal for that organisation, and nothing for any other.

An organisation API token from the portal (**Developer → API Tokens**) also works, sent as `Authorization: Bearer <token>`, for clients that can't do OAuth.

## Before you connect

- You need an ASI Cloud self-service account. You can create one at sign-in, or at https://cloud.asi.nz/portal/register.
- Deploying VMs, creating S3 buckets and turning on AI Inference cost money, and need a payment method saved in the portal under **Billing**. The agent is told to ask you before anything that costs money, deletes data or stops access.
- Services are hosted in New Zealand only. The S3 endpoint accepts connections from New Zealand IP addresses only.
- Secrets (S3 keys, AI Inference keys) are shown once when created, so they appear in the conversation. They are never listed again.

## What agents can do

| Category | Capabilities |
| --- | --- |
| Virtual machines | List VMs with power state, IPs and cost; deploy from Linux or Windows templates; power on or off, rename, resize, snapshot, restore and delete |
| Networking | List, add and remove port-forwarding firewall rules, limited to the source addresses you choose |
| SSH keys | List, upload and delete public keys for new Linux VMs |
| S3 storage | Create, empty and delete buckets; rotate keys; set permissions and IP allow-lists |
| AI Inference | Turn it on or off; create, revoke and delete API keys |
| Billing | Month-to-date and past bills by service, prices |
| Support | List, read and open support tickets with ASI |
| IT products | Search and price IT products ASI sells, in NZD |

The hosted server is the source of truth for tool names and schemas.

## Disconnect

Remove the connection in the ASI Cloud portal under **Developer → API Tokens → Connected apps**, which revokes it straight away, or remove the account from the plugin in your client.

## Docs

- ASI Cloud developer docs: https://cloud.asi.nz/docs
- Server URL: https://cloud.asi.nz/mcp
- Support: https://cloud.asi.nz/support
- Privacy policy: https://cloud.asi.nz/legal/privacy
- Terms: https://cloud.asi.nz/legal/terms

Logo is ASI Solutions' official mark.

## License

MIT
