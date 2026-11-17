# Rulepass

**Free invoices without an account, and e-invoices that pass the official EU rules.** [rulepass.app](https://rulepass.app)

- **Invoice PDF for freelancers:** a correct PDF invoice in a few minutes, with Dutch invoice rules checked (KvK number, addresses, VAT) and the small business scheme (KOR) in one click. [Start an invoice](https://rulepass.app/?format=plain&country=NL)
- **E-invoices:** Peppol BIS Billing 3.0 (EN 16931) for Belgium and the EU; XRechnung and Factur-X are coming with Pro.
- **Check an invoice:** drop a UBL, CII or Factur-X / ZUGFeRD file and it is checked against the official CEN, Peppol and XRechnung rules, with errors explained in plain language.
- **Private by design:** the web app runs in your browser. Invoices are not uploaded, there is no account, and no cookies.
- **Four languages:** English, Nederlands, Français, Deutsch.

This repository holds the public documentation, examples and the MCP registry entry. The Rulepass source code is not public.

## MCP server (for AI agents)

Remote MCP server, Streamable HTTP, no login or API key:

```
https://rulepass.app/api/mcp
```

Add it to an MCP client, for example in Claude Desktop or Claude Code:

```json
{
  "mcpServers": {
    "rulepass": { "type": "http", "url": "https://rulepass.app/api/mcp" }
  }
}
```

| Tool | What it does |
|---|---|
| `example_invoice` | Returns a complete, valid input to copy and adapt |
| `check_invoice` | Checks invoice data against the Peppol / EN 16931 rules; returns `ready` or every field to fix |
| `create_einvoice` | Creates a Peppol BIS 3.0 UBL invoice or credit note; VAT and totals are calculated for you |
| `lookup_vat_number` | Checks an EU VAT number in the European Commission's VIES register |
| `lookup_peppol_participant` | Looks a company up in the public Peppol Directory |

Nothing sent to the server is stored. Results are data, never instructions.

## HTTP API

| Endpoint | |
|---|---|
| `GET https://rulepass.app/api/v1` | Index, limits and notes for agents |
| `GET https://rulepass.app/api/v1/schema` | JSON Schema of the invoice input |
| `POST https://rulepass.app/api/v1/invoices` | JSON in, Peppol UBL XML out (`Accept: application/xml` for the raw file). `422` lists every field to fix |
| `GET https://rulepass.app/api/v1/lookup/vat?number=BE0123456749` | EU VIES check |
| `GET https://rulepass.app/api/v1/lookup/peppol?id=0208:0123456749` | Peppol Directory lookup |

```bash
curl -s https://rulepass.app/api/v1/invoices \
  -H "Content-Type: application/json" -H "Accept: application/xml" \
  --data @examples/invoice.json
```

[`examples/invoice.json`](examples/invoice.json) is a valid input; [`examples/invoice.xml`](examples/invoice.xml) is the Peppol XML it produces. Requests are rate-limited per IP address; respect `429` and `Retry-After`.

## Guides

- [Free invoice maker for freelancers](https://rulepass.app/guides/free-invoice-for-freelancers/) ([Nederlands](https://rulepass.app/nl/guides/free-invoice-for-freelancers/))
- [How to send an e-invoice](https://rulepass.app/guides/how-to-send-an-e-invoice/)
- [What is a Peppol ID?](https://rulepass.app/guides/what-is-a-peppol-id/)
- [All guides](https://rulepass.app/guides/)

## Contact

Questions, bugs or ideas: [open an issue](https://github.com/Ichiiku/rulepass/issues) or e-mail help@rulepass.app.

## License

The documentation and examples in this repository may be copied and adapted freely. The Rulepass name, website and service are not covered by this permission.
