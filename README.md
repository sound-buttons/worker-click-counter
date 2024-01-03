# Click Counter

<https://view-counter-sb.jim60105.workers.dev>

This tool is used to calculate the total number of button triggers.

A counter implemented using Cloudflare Workers Edge storage solution.

> [!Note]  
> **The following are the differences from upstream (陳鈞 jim60105)**
>
> - Response text number instead of badge with the GET request.
> - Only step the count with the POST request.
>
> **The following are the differences from upstream (Aveek Saha)**
>
> - Change from KV storage to D1 database.
> - Migrate project from JavaScript to TypeScript.
> - Migrate from Service Workers to ES Modules (D1 can only works with ES Modules).
> - Update CI workflow.

> [!Important]
> It is important to note that _D1 is currently in public beta_.  
> It is not advisable to utilize beta products for large production workloads.  
> If you find yourself in this scenario, please choose to use the upstream KV solution.  
> Also, please star🌟 and watch👀 this repo to stay updated with our future modifications.

## Setup

The view counter badge is meant to be deployed individually for each profile/user. Click the button and then follow the steps below to deploy your own view counter. During the process, it will guide you to log in (or register) your GitHub and Cloudflare accounts.

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/jim60105/worker-view-counter-badge)

After completing the process, please return to GitHub. At this point, the Action will fail to deploy, so it is essential to proceed with the following configuration.

### Set your worker name

You can change the name in the first line of `wrangler.toml`, as it will serve as the worker name and is related to the subdomain.

Your Cloudflare Worker will be deployed at `{worker-name}.{cloudflare-id}.workers.dev`.

### Prepare your new D1 Database

Create a new D1 Database, it's important that you name it `ViewCounter`.

After obtaining the `database_id` from the execution result of the command, please insert it back into the `wrangler.toml` file and save the file. Following these steps:

#### Step 1: Create a new D1 Database

> [!IMPORTANT]  
> Execute the following command in your working directory.

Create a new D1 Database with the name `ViewCounter`.

```bash
wrangler d1 create ViewCounter
```

You will get a response with the `database_id` like this:

```bash
✅ Successfully created DB 'ViewCounter' in region APAC
Created your database using D1's new storage backend. The new storage backend is not yet recommended for production
workloads, but backs up your data via point-in-time restore.

[[d1_databases]]
binding = "DB" # i.e. available in your Worker on env.DB
database_name = "ViewCounter"
database_id = "72f5fd97-3f01-40e7-b31f-6c3117a9c624"
```

#### Step 2: Update `wrangler.toml`

Fill the `database_id` property in `wrangler.toml` with the id you got in the previous step.

> [!IMPORTANT]  
> `binding` and `database_name` must be `ViewCounter` for our code to work.

```toml
[[d1_databases]]
binding = "ViewCounter"
database_id = "72f5fd97-3f01-40e7-b31f-6c3117a9c624" # Your database id
database_name = "ViewCounter"
```

#### Step 3: Create the table

Create a table named `ViewCounter` with `init_database.sql`

```bash
wrangler d1 execute ViewCounter --file=./init_database.sql
```

## Deploy

Save the file and push a new commit into `master` and wait for the GitHub Action to deploy your worker.

## License

MIT License  
Copyright (c) 2022 Aveek Saha and 陳鈞

[![CodeFactor](https://www.codefactor.io/repository/github/jim60105/worker-view-counter-badge/badge)](https://www.codefactor.io/repository/github/jim60105/worker-view-counter-badge)
