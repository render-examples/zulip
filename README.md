# Deploy Zulip on Render

[Zulip](https://zulip.com/) is an open-source real-time chat application with an email threading model. You can easily deploy it on Render using this repo.

The following components will be created:

- Zulip web/application server which contains:
  - Nginx: front-end web server that serves static assets and proxies to Django and Tornado
  - Django: main web application server
  - Tornado: asynchronous server for maintaining persistent connection from every running client
  - Supervisor: Monitors and maintains all server processes
- Memcached instance: used to cache database model objects
- Redis instance: used for short-term data stores
- RabbitMQ instance: used as a queuing system and to communicate between Django and Tornado
- PostgreSQL instance: used to store all persistent data

More details on Zulip’s architecture here(https://zulip.readthedocs.io/en/latest/overview/architecture-overview.html).

## Deployment

Use the button below to deploy Zulip on Render with 1 click.

[![Deploy to Render](http://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)

You can also follow a manual deployment process using our guide(# TODO: insert link).

### Pricing

This repo launches 5 total services:
- 4 starter and 1 standard plus (for the Zulip server): $78/month
- 4 persistent disks of up to 10GB each: up to $10/month based on the amount of storage used ($.25 per GB per month)

Zulip recommends deploying the main application/webserver with more than 2GB RAM (ideally 4). The standard plus service gives you 3 GB of RAM and 1.5 CPU, but we recommend upgrading to the pro service for production use. Zulip has [documentation](https://zulip.readthedocs.io/en/stable/production/requirements.html#scalability) on how resource needs scale with # of users on the app.

### Environment Variables

To test Zulip out, you only need to set 1 environment variable:
- `SETTING_ZULIP_ADMINISTRATOR`: Set this to your personal email address or the email address of the Zulip administrator

You will be able to set up your account and use all of Zulip's features as a single user. Inviting other users to join your realm (Zulip's term for organization) requires setting up email-related environment variables, which are explained below.

#### Production

To send emails to users, Zulip requires an email server. Properly sending mail can be tricky, so we've omitted a mail server from the deployment and strongly recommend using a third-party email provider.

See the official [Zulip recommended email providers](https://zulip.readthedocs.io/en/latest/production/email.html?highlight=email#email-services).

We have tested this repo successfully with [Mailgun](https://www.mailgun.com/), but other providers in the list above should work equally well.

These are the environment variables you will need to set for production use:
- `SETTING_EXTERNAL_HOST`: This is the hostname your users will use to connect to your Zulip server. Set this to a custom domain or leave it empty to default to the onrender.com domain given to you by Render.
- `SETTING_EMAIL_HOST`: the address of your SMTP provider (e.g. smtp.mailgun.org).
- `SETTING_EMAIL_HOST_USER`: your SMTP provider username (e.g. postmaster@mg.example.com)
- `SETTING_EMAIL_PORT`: the port your SMTP provider uses to connect (e.g. 587).
- `SECRETS_email_password`: your SMTP provider password
- `SETTING_EMAIL_USE_SSL`: set to true if the port you set is using SSL
- `SETTING_EMAIL_USE_TLS`: set to true if the port you set is using TLS

#### Optional

- `ZULIP_AUTH_BACKENDS`: We've set this to `EmailAuthBackend` to support email address & password authentication, but Zulip supports [other authentication methods](https://zulip.readthedocs.io/en/latest/production/authentication-methods.html).

### Setup

Once the `zulip` service is deployed and live, navigate to the `Shell` tab and type in:

```
su zulip
/home/zulip/deployments/current/manage.py generate_realm_creation_link
```

Visit the link outputted by the command and create your Zulip administrator account with the same email you used for `SETTING_ZULIP_ADMINISTRATOR`.
