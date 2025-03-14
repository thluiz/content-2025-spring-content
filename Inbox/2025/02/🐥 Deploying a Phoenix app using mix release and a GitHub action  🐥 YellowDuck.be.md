Here's a transcript of how you can deploy an Elixir Phoenix web application using mix releases and a GitHub action.

The release will be deployed by a systemd unit on a Linux server.

Create a blank app:

```plaintext
$ mix phx.new hello * creating hello/lib/hello/application.ex * creating hello/lib/hello.ex * creating hello/lib/hello_web/controllers/error_json.ex * creating hello/lib/hello_web/endpoint.ex * creating hello/lib/hello_web/router.ex * creating hello/lib/hello_web/telemetry.ex * creating hello/lib/hello_web.ex * creating hello/mix.exs * creating hello/README.md * creating hello/.formatter.exs * creating hello/.gitignore * creating hello/test/support/conn_case.ex * creating hello/test/test_helper.exs * creating hello/test/hello_web/controllers/error_json_test.exs * creating hello/lib/hello/repo.ex * creating hello/priv/repo/migrations/.formatter.exs * creating hello/priv/repo/seeds.exs * creating hello/test/support/data_case.ex * creating hello/lib/hello_web/controllers/error_html.ex * creating hello/test/hello_web/controllers/error_html_test.exs * creating hello/lib/hello_web/components/core_components.ex * creating hello/lib/hello_web/controllers/page_controller.ex * creating hello/lib/hello_web/controllers/page_html.ex * creating hello/lib/hello_web/controllers/page_html/home.html.heex * creating hello/test/hello_web/controllers/page_controller_test.exs * creating hello/lib/hello_web/components/layouts/root.html.heex * creating hello/lib/hello_web/components/layouts/app.html.heex * creating hello/lib/hello_web/components/layouts.ex * creating hello/priv/static/images/logo.svg * creating hello/lib/hello/mailer.ex * creating hello/lib/hello_web/gettext.ex * creating hello/priv/gettext/en/LC_MESSAGES/errors.po * creating hello/priv/gettext/errors.pot * creating hello/priv/static/robots.txt * creating hello/priv/static/favicon.ico * creating hello/assets/js/app.js * creating hello/assets/vendor/topbar.js * creating hello/assets/css/app.css * creating hello/assets/tailwind.config.js Fetch and install dependencies? [Yn] y * running mix deps.get * running mix assets.setup * running mix deps.compile We are almost there! The following steps are missing: $ cd hello Then configure your database in config/dev.exs and run: $ mix ecto.create Start your Phoenix app with: $ mix phx.server You can also run your app inside IEx (Interactive Elixir) as: $ iex -S mix phx.server
```

Generate the release files:

```plaintext
$ mix phx.gen.release * creating rel/overlays/bin/server * creating rel/overlays/bin/server.bat * creating rel/overlays/bin/migrate * creating rel/overlays/bin/migrate.bat * creating lib/hello/release.ex Your application is ready to be deployed in a release! See https://hexdocs.pm/mix/Mix.Tasks.Release.html for more information about Elixir releases. Here are some useful release commands you can run in any release environment: # To build a release mix release # To start your system with the Phoenix server running _build/dev/rel/hello/bin/server # To run migrations _build/dev/rel/hello/bin/migrate Once the release is running you can connect to it remotely: _build/dev/rel/hello/bin/hello remote To list all commands: _build/dev/rel/hello/bin/hello
```

Create the folder structure on the target server:

```plaintext
mkdir -p /var/www/hello.yellowduck.be
```

Generate a new secret for the deployment:

```plaintext
mix phx.gen.secret
```

Create the `.env` file on the target server:

_/var/www/hello.yellowduck.be/.env_

```bash
PHX_HOST=hello.yellowduck.be PORT=4001 PHX_SERVER=true SECRET_KEY_BASE=my-secret-key MIX_ENV=prod DATABASE_URL=ecto://user:pass@localhost/hello
```

Create the systemctl daemon:

_/etc/systemd/system/hello-yellowduck-be.service_

```plaintext
[Unit] Description=hello.yellowduck.be [Service] User=root EnvironmentFile=/var/www/hello.yellowduck.be/.env Environment=LANG=en_US.utf8 WorkingDirectory=/var/www/hello.yellowduck.be/ ExecStart=/var/www/hello.yellowduck.be/_build/prod/rel/hello/bin/hello start ExecStop=/var/www/hello.yellowduck.be/_build/prod/rel/hello/bin/hello stop KillMode=process Restart=on-failure LimitNOFILE=65535 SyslogIdentifier=hello-yellowduck-be [Install] WantedBy=multi-user.target
```

Load the daemon configuration:

```plaintext
systemctl daemon-reload
```

Define the following secrets in GitHub:

-   `SSH_KEY`: the SSH key used to connect to the server (see [here on how to get this info](https://github.com/appleboy/ssh-action#copy-rsa-private-key))
-   `SSH_HOST`: the hostname of the server to where you want to deploy
-   `SSH_USER`: the username you you want to use to connect via SSH to the server

Create the github action (remember to update the env vars at the top of the action with the correct values for your environment):

_.github/workflows/deploy.yaml_

```yaml
name: Deploy on: push: branches: ["main"] pull_request: branches: ["main"] env: MIX_ENV: prod UBUNTU_VERSION: ubuntu-20.04 DEPLOY_PATH: /var/www/hello.yellowduck.be DEPLOY_APP_NAME: hello DEPLOY_DAEMON_NAME: hello-yellowduck-be permissions: contents: write jobs: deploy: runs-on: ubuntu-20.04 steps: - name: Checkout code uses: actions/checkout@v4 - name: Set up Elixir uses: erlef/setup-beam@v1 with: version-file: .tool-versions version-type: strict - name: Cache deps id: cache-deps uses: actions/cache@v4 env: cache-name: cache-elixir-deps with: path: deps key: $&#123;&#123; env.UBUNTU_VERSION &#124;&#124;-mix-$&#123;&#123; env.cache-name &#124;&#124;-$&#123;&#123; hashFiles('**/mix.lock') &#124;&#124;-$&#123;&#123; hashFiles('**/.tool-versions') &#124;&#124; restore-keys: | $&#123;&#123; env.UBUNTU_VERSION &#124;&#124;-mix-$&#123;&#123; env.cache-name &#124;&#124;- - name: Cache compiled build id: cache-build uses: actions/cache@v4 env: cache-name: cache-compiled-build with: path: _build key: $&#123;&#123; env.UBUNTU_VERSION &#124;&#124;-mix-$&#123;&#123; env.cache-name &#124;&#124;-$&#123;&#123; hashFiles('**/mix.lock') &#124;&#124;-$&#123;&#123; hashFiles('**/.tool-versions') &#124;&#124; restore-keys: | $&#123;&#123; env.UBUNTU_VERSION &#124;&#124;-mix-$&#123;&#123; env.cache-name &#124;&#124;- $&#123;&#123; env.UBUNTU_VERSION &#124;&#124;-mix- - name: Clean to rule out incremental build as a source of flakiness if: github.run_attempt != '1' run: | mix deps.clean --all mix clean shell: sh - name: Install dependencies run: mix deps.get --only-prod - name: Compile run: mix compile - name: Compile assets run: mix assets.deploy - name: Compile release run: mix release --overwrite - name: Install SSH key uses: shimataro/ssh-key-action@v2 with: key: $&#123;&#123; secrets.SSH_KEY &#124;&#124; known_hosts: 'to be defined on next step' - name: Add Known Hosts run: ssh-keyscan -H $&#123;&#123; secrets.SSH_HOST &#124;&#124; >> ~/.ssh/known_hosts - name: Deploy Release with rsync run: rsync --delete -avz ./_build $&#123;&#123; secrets.SSH_USER &#124;&#124;@$&#123;&#123; secrets.SSH_HOST &#124;&#124;:$&#123;&#123; env.DEPLOY_PATH &#124;&#124; - name: Restart Application uses: appleboy/ssh-action@v1.2.0 with: host: $&#123;&#123; secrets.SSH_HOST &#124;&#124; username: $&#123;&#123; secrets.SSH_USER &#124;&#124; key: $&#123;&#123; secrets.SSH_KEY &#124;&#124; script: | export $(cat $&#123;&#123; env.DEPLOY_PATH &#124;&#124;/.env | xargs) && $&#123;&#123; env.DEPLOY_PATH &#124;&#124;/_build/$&#123;&#123; env.MIX_ENV &#124;&#124;/rel/$&#123;&#123; env.DEPLOY_APP_NAME &#124;&#124;/bin/migrate systemctl daemon-reload systemctl restart $&#123;&#123; env.DEPLOY_DAEMON_NAME &#124;&#124;
```

Configure Caddy to expose the application:

```plaintext
$ vim /etc/caddy/Caddyfile
```

Then add the following entry:

```plaintext
hello.yellowduck.be { root * /var/www/hello.yellowduck.be encode zstd gzip reverse_proxy localhost:4001 header -Server log { output file /var/log/caddy/access_hello_yellowduck.log } }
```

When you now push anything to the `main` branch, it will be built as a release, transferred to the target server and it will restart the daemon.

<small>If this post was enjoyable or useful for you, <strong>please share it</strong>! If you have comments, questions, or feedback, you can email my <a href="https://mail.google.com/mail/?view=cm&amp;fs=1&amp;tf=1&amp;to=pieter@yellowduck.be" target="_blank">personal email</a>. To get new posts, subscribe use <a href="https://www.yellowduck.be/posts/feed">the RSS feed</a>.</small>