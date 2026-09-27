---
title: Build your first AI Agent with Gemini, n8n and Google Cloud Run
link: https://www.philschmid.de/n8n-cloud-run-gemini
source: philschmid-de
published: 2025-10-30T00:00:00Z
updated: 2025-10-30T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to deploy n8n on Google Cloud Run with PostgreSQL and create an AI Agent using Google Gemini 2.5.
content: extracted
html: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.html
preview:
  file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.preview-7be8efa18f1f.webp
  width: 256
  height: 128
  alt: Build your first AI Agent with Gemini, n8n and Google Cloud Run
  color: '#373737'
images:
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/thumbnail.jpg
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-f923238aeb78.jpg
    width: 1200
    height: 600
  color: '#2e2e2e'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/n8n.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-8a0f5e2b2a7c.png
    width: 1609
    height: 1252
  color: '#2d2d2d'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/api-key.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-118be3aa7810.png
    width: 1143
    height: 801
  color: '#333334'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-editor.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-4deceb2a9b04.png
    width: 1476
    height: 1046
  color: '#333334'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-a7682ceced56.png
    width: 785
    height: 518
  color: '#2e2e2e'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-chat.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-3be8079f3e2a.png
    width: 1409
    height: 814
  color: '#2e2e2e'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-mcp.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-679c51c9c575.png
    width: 705
    height: 501
  color: '#2e2e2e'
- source: https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-mcp-chat.png
  original:
    file: 2025-10-30-build-your-first-ai-agent-with-gemini-n8n-and-google-cloud.image-31b811695370.png
    width: 1405
    height: 961
  color: '#2e2e2e'
---

[n8n](https://n8n.io/) is a powerful, open-source workflow automation tool that allows you to connect and automate different services and applications. This guide will walk you through deploying n8n on Google Cloud Run with a PostgreSQL database for persistent storage and creating a simple AI Agent using Google Gemini 2.5.

Before we start you need a Google Cloud Platform (GCP) account with a billing account configured.

## Step 1: Install and Configure gcloud CLI

To interact with Google Cloud from your terminal, you need the Google Cloud CLI.

1. **Install the gcloud CLI:** You can install the Google Cloud CLI using the following command, or follow the [official installation instructions](https://cloud.google.com/sdk/docs/install) for your specific operating system.

   Bash

   ```bash
   curl https://sdk.cloud.google.com | bash
   exec -l $SHELL
   gcloud init
   ```

   Run `gcloud version` to verify the installation.

2. **Login to Google Cloud:** Once installed, authenticate with your Google Cloud account:

   Bash

   ```bash
   gcloud auth login
   ```

   A browser window will open asking you to log in and allow the Google Cloud SDK to access your account. Copy the authentication code and paste it into the terminal.

## Step 2: Setup Google Cloud Project

We need to set up a new Google Cloud project and enable the necessary services.

1. **Create a new project:**

   Bash

   ```bash
   export PROJECT_ID="n8n-gemini-$(head /dev/urandom | tr -dc a-z0-9 | head -c 10)"
   gcloud projects create "$PROJECT_ID" --name="n8n Gemini Quickstart"
   gcloud config set project "$PROJECT_ID"
   ```

2. **Link Billing Account:** You need to enable billing for your new project. Run the command below and open the URL in your browser to link your billing account.

   Bash

   ```bash
   echo "https://console.cloud.google.com/billing/linkedaccount?project=$PROJECT_ID"
   ```

3. **Set environment variables:**

   Bash

   ```bash
   export REGION="us-central1" # Change to your preferred region, e.g. us-east1, us-west1, etc.
   ```

4. **Enable necessary APIs:**

   Bash

   ```bash
   gcloud services enable run.googleapis.com \
       sqladmin.googleapis.com \
       secretmanager.googleapis.com \
       iam.googleapis.com
   ```

## Step 3: Setup Database and Secrets

n8n requires a database to store its data. We'll use Cloud SQL for PostgreSQL and Secret Manager to securely store credentials. We are using a `db-f1-micro` tier for cost-effectiveness, but you can adjust the parameters like tier, storage size, and region based on your needs. For more configuration options, refer to the [official documentation](https://docs.cloud.google.com/sql/docs/postgres/create-instance).

*Note: This can take ~10-15 minutes.*

Bash

```bash
export N8N_DB_PASSWORD=$(openssl rand -base64 16)
export N8N_ENCRYPTION_KEY=$(openssl rand -base64 42)
 
# Create Cloud SQL instance
gcloud sql instances create n8n-db \
    --database-version=POSTGRES_13 \
    --tier=db-f1-micro \
    --region=$REGION \
    --root-password=$N8N_DB_PASSWORD \
    --storage-size=10GB \
    --no-backup \
    --storage-type=HDD
 
# Create database and user
gcloud sql databases create n8n --instance=n8n-db
gcloud sql users create n8n-user \
    --instance=n8n-db \
    --password=$N8N_DB_PASSWORD
 
# Store secrets in Secret Manager
echo $N8N_DB_PASSWORD | gcloud secrets create n8n-db-password --data-file=- --replication-policy="automatic"
echo $N8N_ENCRYPTION_KEY | gcloud secrets create n8n-encryption-key --data-file=- --replication-policy="automatic"
```

## Step 4: Deploy n8n to Cloud Run

Now we'll deploy n8n to Cloud Run, connecting it to our database and secrets. We need to create a service account for n8n and grant it access to the secrets, then deploy the n8n container to Cloud Run with the necessary environment variables and secrets.

1. **Create a service account for n8n:**

   Bash

   ```bash
   gcloud iam service-accounts create n8n-service-account \
       --display-name="n8n Service Account"
   export SERVICE_ACCOUNT_EMAIL="n8n-service-account@$PROJECT_ID.iam.gserviceaccount.com"
   ```

2. **Grant necessary permissions:**

   Bash

   ```bash
   gcloud secrets add-iam-policy-binding n8n-db-password \
       --member="serviceAccount:$SERVICE_ACCOUNT_EMAIL" \
       --role="roles/secretmanager.secretAccessor"
   gcloud secrets add-iam-policy-binding n8n-encryption-key \
       --member="serviceAccount:$SERVICE_ACCOUNT_EMAIL" \
       --role="roles/secretmanager.secretAccessor"
   gcloud projects add-iam-policy-binding $PROJECT_ID \
       --member="serviceAccount:$SERVICE_ACCOUNT_EMAIL" \
       --role="roles/cloudsql.client"
   ```

3. **Deploy to Cloud Run:**

   Bash

   ```bash
   DB_CONNECTION_NAME="$PROJECT_ID:$REGION:n8n-db"

   gcloud run deploy n8n \
       --image=n8nio/n8n:latest \
       --region=$REGION \
       --allow-unauthenticated \
       --port=5678 \
       --memory=2Gi \
       --cpu=1 \
       --no-cpu-throttling \
       --set-env-vars="N8N_PORT=5678,N8N_PROTOCOL=https,DB_TYPE=postgresdb,DB_POSTGRESDB_DATABASE=n8n,DB_POSTGRESDB_USER=n8n-user,DB_POSTGRESDB_HOST=/cloudsql/$DB_CONNECTION_NAME,DB_POSTGRESDB_PORT=5432,DB_POSTGRESDB_SCHEMA=public,GENERIC_TIMEZONE=UTC,QUEUE_HEALTH_CHECK_ACTIVE=true" \
       --set-secrets="DB_POSTGRESDB_PASSWORD=n8n-db-password:latest,N8N_ENCRYPTION_KEY=n8n-encryption-key:latest" \
       --add-cloudsql-instances=$DB_CONNECTION_NAME \
       --service-account=$SERVICE_ACCOUNT_EMAIL
   ```

   *Note: We use `--no-cpu-throttling` to ensure n8n's background processes (like schedules and wait nodes) continue running even when not actively handling an HTTP request.*

After a successful deployment, Cloud Run will output the URL of your n8n instance. This should look like:

```plaintext
Done.
Service [n8n] revision [n8n-00001-qqn] has been deployed and is serving 100 percent of traffic.
Service URL: https://n8n-780597096402.us-central1.run.app
```

## Step 5: Create your first Gemini Agent

After you access your n8n instance for the first time, you will be prompted to set up an owner account. Follow the instructions to create an owner account. Here you can also add your enterprise license if you have one. Awesome, once done you should see a screen like this:

![n8n](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/n8n.png)

We are going to create an Agent from scratch. But before we create the Agent we need to add our Gemini API Key. Click on "Credentials" and then "Add first credential" and search for "Google Gemini (PaLM) API". Click on "Create" and follow the instructions to create a new credential. If you don't have a Gemini API Key, you can get one from the [AI Studio](https://aistudio.google.com/app/api-keys).

![api-key](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/api-key.png)

Once validated go back to "Workflows" and click on "Start from scratch". You should now land in the "editor". Click "Add first step" and search for "AI Agent". Now, you should be in the in configuration of the AI Agent. At the bottom in the middle of the screen you should see "Chat Model", "Memory" and "Tool". Click on "Chat Model"

![agent-editor](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-editor.png)

Then search for "Google Gemini Chat Model". There you should be able to select your credentials and the model you want to use. Once defined click "Back to canvas" (top left corner). You should now see a "Basic AI Agent" with Gemini.

![agent](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent.png)

Lets try it! Click on "Open Chat" and ask it a question.

![agent-chat](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-chat.png)

Awesome, it works! Now, lets add some tools and make it an Agent. Therefore click on "+" at "tool" under the AI Agent. Now, you should see a list of tools you can add to your Agent, including MCP servers, vector databases, or hundreds of other tools.

Let's try to add the "MCP (Model Context Protocol) Client". MCP is a standard that allows LLMs to connect to external data sources without custom integration code. We will use it to connect Gemini to a dummy weather service. As endpoint add `https://gemini-api-demos.uc.r.appspot.com/mcp` (Note: There is no guarantee that this endpoint is always available.) Once you have added your MCP server click on "back to canvas".

Your Agent should now have a tool connection to a "MCP Client".

![agent-mcp](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-mcp.png)

Okay, lets test it. Click on "Open Chat" and ask it about the weather in New York, you have to include the "date" in your question, e.g. `What is the weather in New York on 2025-10-29?`

![agent-mcp-chat](https://www.philschmid.de/static/blog/n8n-cloud-run-gemini/agent-mcp-chat.png)

Awesome, it works! During the execution should have seen the Agent first calling Gemini, which outputs a structured call, to call the MCP Server, the MCP Server response was then sent back to Gemini, which generates the final response.

Don't forget to delete your Google Cloud resources when you are done. You can delete the resources using the following command:

Bash

```bash
gcloud run services delete n8n --region=$REGION 
gcloud sql instances delete n8n-db 
gcloud secrets delete n8n-db-password 
gcloud secrets delete n8n-encryption-key 
gcloud iam service-accounts delete $SERVICE_ACCOUNT_EMAIL 
gcloud projects delete $PROJECT_ID 
```

## Next Steps

Congratz! You have successfully deployed n8n to Google Cloud Run and created your first AI Agent using Gemini 2.5. Now it's time to explore more advanced features, add more tools and build complex workflows to automate your processes. As you move from a proof-of-concept to production, consider these resources:

- **Community Workflows**: [Over 600 community build Gemini n8n workflows](https://n8n.io/workflows/?integrations=Google+Gemini+Chat+Model)
- **Scaling n8n**: For high-load setups, look into [n8n's Queue Mode](https://docs.n8n.io/hosting/scaling/), which separates the main process from execution workers.
- **Infrastructure Optimization**: Optimize [concurrency](https://cloud.google.com/run/docs/about-concurrency), [startup CPU boost](https://cloud.google.com/run/docs/configuring/services/cpu#startup-boost), [database](https://docs.n8n.io/hosting/execution-data/#pruning-execution-data)
- **Advanced AI Nodes**: Explore n8n's [advanced AI documentation](https://docs.n8n.io/advanced-ai/) to learn about different types of memory, output parsers, and custom tools.
- **Human-in-the-loop**: specific nodes can be used to ask for [human approval](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.wait/) before executing sensitive actions.
- [**n8n Community Forum**](https://community.n8n.io): The best place to ask questions, share your workflows, and learn from others.
- [**n8n YouTube Channel**](https://www.youtube.com/@n8n-io)\*\*: Great for visual tutorials and deep dives into complex use cases.
