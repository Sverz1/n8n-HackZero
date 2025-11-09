# n8n-HackZero - Deploy n8n on Render

This repository contains the configuration to deploy n8n workflow automation platform on Render.com.

## Quick Deploy

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)

## What Gets Deployed

This Blueprint creates:
1. **n8n Web Service** - The n8n workflow automation platform
2. **PostgreSQL Database** - For persistent storage of workflows and credentials
3. **Persistent Disk** - 1GB disk for n8n data storage

## Required Environment Variables

When deploying, you'll need to provide:

### Required (You Must Set These):

1. **WEBHOOK_URL**
   - Your full Render service URL
   - Format: `https://n8n-hackzero.onrender.com`
   - ⚠️ You'll get this URL after first deployment
   - ⚠️ You'll need to update it in Render Dashboard after deployment

2. **N8N_BASIC_AUTH_USER**
   - Username for n8n login
   - Example: `admin` or your preferred username

3. **N8N_BASIC_AUTH_PASSWORD**
   - Strong password for n8n login
   - Example: Use a password manager to generate a secure password

### Auto-Generated (Render Handles These):

- **DATABASE_URL** - Automatically connected to PostgreSQL
- **N8N_ENCRYPTION_KEY** - Auto-generated secure key
- **PORT** - Set to 5678 (n8n default)
- **DB_TYPE** - Set to postgresdb

## Deployment Steps

### Step 1: Deploy the Blueprint

1. Click the "Deploy to Render" button above
2. Or go to [Render Dashboard](https://dashboard.render.com) → **New** → **Blueprint**
3. Connect this repository: `https://github.com/Sverz1/n8n-HackZero`

### Step 2: Configure Environment Variables

During deployment, provide:
- **N8N_BASIC_AUTH_USER**: Your desired username
- **N8N_BASIC_AUTH_PASSWORD**: Your secure password
- **WEBHOOK_URL**: Leave empty for now (you'll update after deployment)

### Step 3: Complete First Deployment

1. Click "Apply" to start deployment
2. Wait 5-10 minutes for:
   - PostgreSQL database creation
   - n8n service deployment
   - Disk attachment

### Step 4: Update Webhook URL

After deployment completes:

1. Go to your n8n service in Render Dashboard
2. Copy your service URL (e.g., `https://n8n-hackzero.onrender.com`)
3. Go to **Environment** tab
4. Update `WEBHOOK_URL` with your full service URL
5. Click **Save Changes**
6. Service will redeploy automatically

### Step 5: Access n8n

1. Visit your service URL
2. Login with your `N8N_BASIC_AUTH_USER` and `N8N_BASIC_AUTH_PASSWORD`
3. Start creating workflows!

## Generate API Key for MCP Integration

After deploying n8n, you'll need to generate an API key:

1. Login to your n8n instance
2. Click your user icon (top right) → **Settings**
3. Go to **API** section
4. Click **Create API Key**
5. Copy the generated key

## Update MCP Configuration

Add n8n to your project's `.mcp.json`:

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["n8n-mcp"],
      "env": {
        "MCP_MODE": "stdio",
        "LOG_LEVEL": "error",
        "DISABLE_CONSOLE_OUTPUT": "true",
        "N8N_API_URL": "https://n8n-hackzero.onrender.com",
        "N8N_API_KEY": "your-generated-api-key-here"
      }
    }
  }
}
```

Replace:
- `N8N_API_URL` with your actual Render service URL
- `N8N_API_KEY` with the API key generated in n8n

## Important Notes

### Free Tier Limitations

- **PostgreSQL**: Free for 90 days, then requires paid plan (~$7/month)
- **Web Service**: Free tier spins down after 15 minutes of inactivity
  - First request after spin down may take 30-60 seconds
  - Consider upgrading to paid tier ($7/month) for always-on service
- **Disk**: 1GB included

### Upgrade Recommendations

For production use, consider:
- **Standard Plan** ($7/month): Always-on service, no spin down
- **PostgreSQL** ($7/month): Continuous database availability
- **Total**: ~$14/month for production-ready n8n

### Webhook Behavior on Free Tier

⚠️ **Important**: Free tier services spin down, which affects webhooks:
- External webhooks may fail during spin down
- First webhook after spin down will take 30-60 seconds
- For reliable webhooks, upgrade to paid plan

## Troubleshooting

### Service Won't Start

- Check Render logs in Dashboard
- Verify all environment variables are set
- Ensure PostgreSQL database is running

### Can't Login

- Verify `N8N_BASIC_AUTH_USER` and `N8N_BASIC_AUTH_PASSWORD` are set correctly
- Check service logs for authentication errors

### Webhooks Not Working

- Verify `WEBHOOK_URL` matches your actual service URL
- Check n8n webhook settings in workflow
- For free tier: Service may be spinning up (wait 30-60 seconds)

### Database Connection Issues

- Verify `DATABASE_URL` is connected to PostgreSQL
- Check PostgreSQL database is running in Render Dashboard
- Review n8n logs for database errors

## Upgrading n8n Version

To upgrade n8n to a newer version:

1. Edit `render.yaml`
2. Change `image.url` from `latest` to specific version:
   ```yaml
   image:
     url: docker.n8n.io/n8nio/n8n:1.68.0
   ```
3. Commit and push changes
4. Render will auto-deploy the new version

## Support

- [n8n Documentation](https://docs.n8n.io/)
- [Render Documentation](https://render.com/docs)
- [n8n Community](https://community.n8n.io/)

## License

n8n is licensed under [Apache 2.0 with Commons Clause](https://github.com/n8n-io/n8n/blob/master/LICENSE.md)
