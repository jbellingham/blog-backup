## This Week I Learned: Monitoring and Observability with Prometheus, Grafana, and fly.io

## This Week I Learned

How to instrument an Elixir/Phoenix app with Prometheus and Grafana, hosted in fly.io.

### Build it in production
Recently I started building a new side project with Elixir and Phoenix, and I had one thing in my mind from a recent [changelog episode](https://changelog.com/news/ma06/visit), "build it in production". So I'm doing just that. I've decided to host the app with fly.io, and so far I am very happy with that choice.

Something that I've developed a keen interest for in the last year or so is monitoring and observability, and so I knew right away that I wanted to instrument this app as soon as possible. There are a bunch of options out there for observability stacks, but as this is a side project, cost is a big factor when considering those options. I ended up going with [Prometheus](https://prometheus.io/) and [Grafana Cloud](https://grafana.com/products/cloud/), because fly.io have a hosted Prometheus instance that I can easily publish to with almost no configuration, and Grafana Cloud because the free tier is free forever and pretty generous, and it plays nicely with my choice of instrumentation library.

### How do observability?
First thing we need to have is a way to publish metrics from our running (or soon to be running) app. [PromEx](https://hex.pm/packages/prom_ex) is an Elixir library that makes it very easy to publish metrics on all kinds of things relating to an app's operation. We can add it to our list of dependencies in `mix.exs`:

```ruby
defp deps do
  [
      ...other deps
      {:prom_ex, "~> 1.7.1"},
      ...other deps
  ]
end
```

With the dependency added, run
```
mix deps.get
```
to fetch the dependency and make it available to our project. We can then run
```
mix prom_ex.gen.config --datasource prometheus
```
which will produce a new file `lib/app_name/prom_ex.ex`. Follow the documentation provided at the top of this file to make the other necessary code changes.
PromEx comes pre-configured to surface metrics with:

 ```ruby
def plugins do
    [
      # PromEx built in plugins
      Plugins.Application,
      Plugins.Beam,
      {Plugins.Phoenix, router: SocialNetworkWeb.Router, endpoint: SocialNetworkWeb.Endpoint},
      Plugins.Ecto,
      Plugins.PhoenixLiveView
    ]
  end
```
Enabling metrics from things like ecto, phoenix, and live view is as easy as un-commenting those lines, and the corresponding lines for their dashboards under

```ruby
def dashboards do
    [
      # PromEx built in Grafana dashboards
      {:prom_ex, "application.json"},
      {:prom_ex, "beam.json"},
      {:prom_ex, "phoenix.json"},
      {:prom_ex, "ecto.json"},
      {:prom_ex, "phoenix_live_view.json"}
    ]
```

There is one other piece of configuration I needed, that isn't present in the current documentation. In the application config, when you are configuring PromEx in your application pipeline, to also specify the `metrics_server` attribute:

```
config :social_network, SocialNetwork.PromEx,
    manual_metrics_start_delay: :no_delay,
    grafana: [
      host: System.get_env("GRAFANA_HOST") || raise("GRAFANA_HOST is required"),
      auth_token: System.get_env("GRAFANA_TOKEN") || raise("GRAFANA_TOKEN is required"),
      upload_dashboards_on_start: true,
      folder_name: "Social Network App Dashboards",
      annotate_app_lifecycle: true
    ],
    metrics_server: [
      port: 4021,
      path: "/metrics", # This is an optional setting and will default to `"/metrics"`
      protocol: :http, # This is an optional setting and will default to `:http`
      pool_size: 5, # This is an optional setting and will default to `5`
      cowboy_opts: [], # This is an optional setting and will default to `[]`
      auth_strategy: :none # This is an optional and will default to `:none`
    ]
```

Discovered thanks to [this blog post](https://community.fly.io/t/custom-app-metrics-not-making-it-to-grafana-cloud/3902/12), pointing to [this piece of documentation](https://hexdocs.pm/prom_ex/1.6.0/PromEx.Config.html). Note that the PromEx config also needs a couple of environment variables to wire it up to grafana to push the dashboards. We don't have the values for these yet so we'll come back to it.

As I mentioned above, fly.io have a hosted Prometheus instance we can publish to with a few lines of config in our `fly.toml`:

```toml
...
[metrics]
port = 4021
path = "/metrics"
...
```
Prometheus will now poll for metrics on the above endpoint, and with that, we've made all the code changes we need.

In order to publish our dashboards to grafana, we need to create an API token for PromEx to use. In the grafana UI, navigate to `/org/apikeys`, and click "Add API key". Give it a name, I've called mine "PromEx", and give it the role "Editor". Specify a time to live for the API key, and click "Add". Make a note of the key that gets generated, as you won't be able to see it again and will have to generate a new one if you lose it.
Using the fly.io command line tool, you'll need to add secrets to your app for the grafana host and api token, something like this:

```
fly secrets set GRAFANA_HOST=<your grafana instance url here>
fly secrets set GRAFANA_TOKEN=<your grafana api token here>
```

We also need to generate an fly api token for grafana to use, which we can do with:
```
flyctl auth token
```
Make note of the token produced by this command. Same as with the last API key, you won't get to see it again.

---

### Pulling data into Grafana

In order for grafana to consume our metrics from prometheus, we need to configure a data source for it. In the grafana UI, navigate to `/datasources`, click "Add data source", and select Prometheus from the list. Give it the name "prometheus", specifically in all lower case, this property needs to match exactly the `datasource_id` from `prom_ex.ex`.
Configure the rest of the data source as follows:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648715258781/CBQETTMCt.png)

Replacing < ORG SLUG > with your fly.io org slug which can be found by running:
```
flyctl orgs list
```
We also need to add a custom HTTP header with the key "Authorization", and the value "Bearer: < FLY API TOKEN >". Click "Save & test" we should see something like this:


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648715865478/pfYASoVLh.png)

Getting the "Data source is working" message above means we're ready to deploy our fly app and start generating some data. After a successful deploy, if we head over to `/dashboards` on the grafana UI we should see a folder called "Social Network App Dashboards":

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648716319751/hrxUbcOwe.png)

Expand the folder and we should see dashboards for all of the options we specified in `prom_ex.ex`:
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648716411582/VvjUuNXLt.png)

And if we open one up, we should see something like this:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648716493240/jozWRGnSt.png)

If it looks like the above, then everything is working correctly 🎉🎉

If you see a whole lot of "No data" across all the dashboards, even after hitting the app with some traffic, something isn't configured quite right. Double check your data source in grafana, and that clicking "Save & test" produces the "Data source is working". Also double check your app secrets in fly, and the PromEx configuration in the app itself.