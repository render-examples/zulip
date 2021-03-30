# Deploy Zulip on Render

This is a sample [Zulip](https://zulip.com/) app, configured for deployment to [Render](https://render.com/). Zulip is an open-source real-time chat application with an email threading model. 

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

Fork this repo and click the button below to deploy.

[![Deploy to Render](http://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/render-examples/zulip)

More details at https://render.com/docs/deploy-zulip.

If you need help, get in touch at https://community.render.com.
