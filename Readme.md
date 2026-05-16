# SSL Certificate Expiry Checker Workflow

A comprehensive n8n workflow that monitors SSL certificate expiry dates across multiple websites and sends alerts when certificates are expiring soon.

## 📋 Features

- **Multi-Domain Monitoring**: Check SSL expiry for multiple websites in one workflow run
- **Configurable Warning Period**: Set custom warning thresholds (default: 30 days)
- **Slack Notifications**: Receive alerts in Slack when certificates are expiring soon
- **Error Handling**: Built-in error checking and handling
- **Days Calculation**: Automatically calculates days until expiry
- **Status Tracking**: Marks certificates as OK or WARNING

## 🔧 Workflow Nodes

1. **Config** - Set websites to monitor and warning days threshold
2. **Split Websites** - Iterate through website list
3. **Check SSL Expiry** - Query SSL certificate data via SSLmate API
4. **Check for Errors** - Validate API response
5. **Process Results** - Calculate expiry days and format data
6. **Is Expiring Soon?** - Determine if alert is needed
7. **Send Slack Alert** - Send notification to Slack channel
8. **Merge Results** - Consolidate results from all branches

## 🚀 Setup Instructions

### 1. Import Workflow to n8n

1. Open n8n at `http://localhost:5678`
2. Go to **Workflows** → **Import from file**
3. Select `ssl-expiry-workflow.json`
4. Click **Import**

### 2. Configure Websites

Edit the **Config** node:
- Update the `websites` array with your domains
- Set `warningDays` to your preferred threshold

Example:
```json
[
  "yourdomain.com",
  "api.yourdomain.com",
  "mail.yourdomain.com"
]
```

### 3. Set Up SSLmate API Credentials

1. Sign up for SSLmate API at https://www.sslmate.com/
2. Get your API credentials
3. In the **Check SSL Expiry** node:
   - Click "Add Credentials"
   - Add your SSLmate API key and secret
   - Select "HTTP Basic Auth" authentication

### 4. Configure Slack Integration

1. Create a Slack bot in your workspace
2. Get the bot's OAuth token
3. In the **Send Slack Alert** node:
   - Click "Add Credentials"
   - Add your Slack bot token
   - Verify the channel name is correct (e.g., `#ssl-alerts`)

### 5. Schedule the Workflow

1. Click **Save** and **Activate** the workflow
2. Set up a trigger (e.g., daily cron schedule):
   - Add a "Cron" trigger node or "Schedule" node
   - Connect it to the **Config** node
   - Set to run daily at a specific time

## 📊 Workflow Output

The workflow produces the following data for each domain:

```json
{
  "domain": "example.com",
  "expiryDate": "2025-06-15",
  "daysUntilExpiry": 47,
  "isExpiringSoon": false,
  "status": "OK"
}
```

## ⚠️ Alert Format

When a certificate is expiring soon, Slack receives:

```
🚨 SSL Certificate Expiring Soon!

Domain: example.com
Expiry Date: 2025-06-15
Days Until Expiry: 15
```

## 🔌 Alternative: Using Node.js SSL Checker

If you prefer not to use SSLmate API, replace the **Check SSL Expiry** node with an HTTP Request using a free service:

```
URL: https://api.o.gg/ssl/example.com
Method: GET
```

## 🛠️ Troubleshooting

### "API Key Invalid" Error
- Verify SSLmate API credentials are correct
- Check API key permissions are enabled

### "Slack Channel Not Found"
- Ensure the Slack channel exists
- Verify bot has permission to post in the channel
- Use channel ID instead of name if necessary

### No Domains Are Being Checked
- Verify the JSON array in Config node is valid JSON
- Check that website domains are formatted correctly
- Ensure Split Websites node is connected properly

## 📅 Scheduling

### Run Daily
```
Cron: 0 9 * * *  (Every day at 9 AM)
```

### Run Weekly
```
Cron: 0 9 * * 1  (Every Monday at 9 AM)
```

### Run Monthly
```
Cron: 0 9 1 * *  (First day of month at 9 AM)
```

## 📝 Example Customizations

### Add Email Notifications
1. Add an Email node after "Is Expiring Soon?"
2. Configure with your email settings
3. Set recipient and message template

### Add Database Logging
1. Add a Database node to store results
2. Log all checks for audit purposes
3. Track historical expiry trends

### Add Multiple Slack Channels
1. Branch "Is Expiring Soon?" to multiple Slack nodes
2. Send critical alerts to #critical-ssl-alerts
3. Send warnings to #ssl-alerts

## 🔐 Security Best Practices

- Store API keys in n8n credentials, not in workflow
- Use environment variables for sensitive data
- Restrict Slack bot to specific channels
- Review audit logs regularly
- Rotate API keys periodically

## 📞 Support

For issues with:
- **n8n**: Check the n8n documentation
- **SSLmate**: Visit https://www.sslmate.com/help
- **Slack Bot**: See Slack API documentation

---

**Last Updated**: April 29, 2026
**Version**: 1.0
