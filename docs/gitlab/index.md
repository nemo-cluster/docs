# Gitlab Fast Recovery

This example describes a Gitlab installation on the bwCloud.

## Prerequisites

Get a cloud vm with at least medium, better large flavor and an external IP address.
Generate a DNS entry with a valid TLD to be able to get a certificate.
Instances which will be only accessed within the university can use an internal IP address and can use intra.uni-freiburg.de for the DNS domain.
Configure security group to allow SSH and HTTP(S).
When using Letsencrypt HTTP needs to be available worldwide.


### Optional get AlmaLinux and Rockylinux on Clouds with CentOS Stream

Use the CentOS Stream image and run the following as root. For Almalinux get migration tool first.

Bash:
```bash
curl https://raw.githubusercontent.com/rocky-linux/rocky-tools/main/migrate2rocky/migrate2rocky.sh -o migrate2rocky.sh
chmod +x migrate2rocky.sh
./migrate2rocky.sh -r
rm migrate2rocky.sh -f
reboot
```

### Install needed Packages

Bash:
```bash
yum install -y curl policycoreutils openssh-server perl
# firewalld optional, when vm security group is insufficient
yum install -y postfix
yum install -y firewalld # optional, when security groups configured
systemctl enable --now postfix.service
```

Configure firewall and postfix if needed and enable services.

### Considerations about SSL

If you install a internal Gitlab instance, which can only be accessed within a internal and separate network, you can consider using HTTP without SSL. SSL only works for external domain names. Letsencrypt needs external IP and DNS entry. Uni Freiburg supports SSL with internal IP addresses and DNS intra.uni-freiburg.de.

Most services like Gitlab runner and pages should work without SSL but should only used for testing, demo or within isolated networks.

### Questions

Will Letsencrypt work with internal IP addresses and intra.uni-freiburg.de?


## Install Gitlab Omnibus

Bash:
```bash
curl -L "https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh" | sudo bash
EXTERNAL_URL="https://gitlab.sub1.uni-freiburg.de" yum install -y gitlab-ee
```

When your OS is not recognized you can try the following:
```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh -o script.rpm.sh
chmod +x script.rpm.sh
os=el dist=8 ./script.rpm.sh
rm script.rpm.sh -f
```

Login name is `root` the password is stored in `/etc/gitlab/initial_root_password`.
Please change within 24h.


## Configure Gitlab Instance

Gitlab is configured in file `/etc/gitlab/gitlab.rb` if not specified otherwise.

If not specified during installation, change the name of your instance.
`/etc/gitlab/gitlab.rb`:
```yaml
external_url 'https://gitlab.sub1.uni-freiburg.de'
```

When using HTTPS set up Letsencrypt. For Uni Freiburg CA see [Gitlab SSL Configuration](https://docs.gitlab.com/omnibus/settings/ssl.html).

`/etc/gitlab/gitlab.rb`:
```yaml
letsencrypt['enable'] = true
letsencrypt['contact_emails'] = ['my.name@rz.uni-freiburg'] # optional
```

Reconfigure Gitlab.
Bash:
```bash
gitlab-ctl reconfigure
```


### Security considerations

After first loggin change the password for user `root`.
Add a second factor.
After registering you phone app, consider adding U2F devices.

Go to "Admin Area", "General" and change following boxes in "Sign-in restrictions":
- check "Enforce two-factor authentication"
- check "Enable admin mode"
- uncheck "Allow password authentication for Git over HTTP(S)"
- uncheck "Sign-up enabled" if you do not want Gitlab instance local users

In box "Visibility and access controls" change "Enabled Git access protocols" to "Only SSH". 


## Configure Access though External Services

### Onmiauth

* Gitlab Documentation: [Generic OpenID Connect](https://docs.gitlab.com/ee/administration/auth/oidc.html)
* Gitlab Documentation: [Gitlab.com](https://docs.gitlab.com/ee/integration/gitlab.html)
* Gitlab Documentation: [Github](https://docs.gitlab.com/ee/integration/github.html)


Omniauth allows Authentication through OpenID-Connect and Services like Giltab.com and Github.

If you want to use Keycloak from bwSFS, Gitlab.com and Github, use this example for configuration

```yaml
gitlab_rails['omniauth_enabled'] = true
gitlab_rails['omniauth_allow_single_sign_on'] = ['openid_connect', 'gitlab', 'github']
gitlab_rails['omniauth_auto_link_user'] = ['openid_connect', 'gitlab', 'github']
gitlab_rails['omniauth_external_providers'] = ['openid_connect', 'gitlab', 'github']
gitlab_rails['omniauth_providers'] = [
  { 'name' => 'openid_connect',
    'label' => 'Keycloak RZ',
    'args' => {
      'name' => 'openid_connect',
      'scope' => ['openid','profile','email'],
      'response_type' => 'code',
      'issuer' => 'https://keycloak.sub2.uni-freiburg.de/auth/realms/rdm-services',
      'discovery' => true,
      'client_auth_method' => 'query',
      'uid_field' => 'sub',
      'send_scope_to_token_endpoint' => 'true',
      'client_options' => {
        'identifier' => '<CLINET_ID>',
        'secret' => '<CLIENT_SECRET>',
        'redirect_uri' => 'https://gitlab.sub1.uni-freiburg.de/users/auth/openid_connect/callback'
      }
    }
  },
  {
    "name" => "gitlab",
    "app_id" => "<APP_ID>",
    "app_secret" => "<APP_SECRET>",
    "args" => { "scope" => "api" }
  },
    {
    "name" => "github",
    "app_id" => "<APP_ID>",
    "app_secret" => "<APP_SECRET>",
    "args" => { "scope" => "user:email" }
  }
]
```

For Keycloak connect you will need to get credentials for `client_options`.
For Gitlab.com and Github connect `app_id` and `app_secret` have to be generated first.
For Gitlab.com see this [document](https://docs.gitlab.com/ee/integration/gitlab.html), for Github refer to this [document](https://docs.gitlab.com/ee/integration/github.html).

### LDAP

* Gitlab Documentation: [LDAP](https://docs.gitlab.com/ee/administration/auth/ldap/)

Example for Uni Freiburg LDAP, add following lines to `/etc/gitlab/gitlab.rb`:

`/etc/gitlab/gitlab.rb`:
```yaml
gitlab_rails['ldap_enabled'] = true
gitlab_rails['prevent_ldap_sign_in'] = false
gitlab_rails['ldap_servers'] = YAML.load <<-'EOS'
  main: # 'main' is the GitLab 'provider ID' of this LDAP server
    label: 'LDAP'
    host: 'ldap.sub3.uni-freiburg.de'
    port: 636
    uid: 'uid'
    encryption: 'simple_tls' # "start_tls" or "simple_tls" or "plain"
    verify_certificates: true
    smartcard_auth: false
    active_directory: false
    allow_username_or_email_login: true
    lowercase_usernames: false
    block_auto_created_users: false
    base: 'dc=service1,dc=uni-freiburg,dc=de'
    user_filter: ''
    attributes:
      username: 'uid'
      email: 'rufPreferredMail'
      name: 'cn'
      first_name: 'givenName'
      last_name: 'sn'
EOS
```

Add LDAP bind information to secrets file.
Bash:
```bash
gitlab-rake gitlab:ldap:secret:edit EDITOR=vim
```

Add your bind information:
```yaml
main:
  bind_dn: 'uid=admin,ou=system'
  password: '<PASSWORD>'
```

To get a verified SSL connection, add the DFN chain to trusted certs directory.

Bash:
```bash
curl https://pki.pca.dfn.de/dfn-ca-global-g2/pub/cacert/chain.txt -o /etc/gitlab/trusted-certs/dfn-chain.crt
```

Reconfigure Gitlab.
Bash:
```bash
gitlab-ctl reconfigure
```

## Using External Object Stores

* Gitlab Documentation: [Object Storage](https://docs.gitlab.com/ee/administration/object_storage.html)

First generate one S3 storage bucket for each service you want to use and generate the access keys and secrets.
In most cases you can configure S3 storage in the corresponding  section of a service or you can use the special s3 storage block to configure most storage buckets in one place.
For the sake of simplicity the following example uses the s3 storage block to configure most s3 buckets.
If you don't want an object storage for a service, just add `false` instead of a bucket name.

```yaml
gitlab_rails['object_store']['enabled'] = true
gitlab_rails['object_store']['connection'] = {
  'provider' => 'AWS',
  'region' => '<AWS_DEFAULT_REGION>',
  'aws_access_key_id' => '<AWS_ACCESS_KEY_ID>',
  'aws_secret_access_key' => '<AWS_SECRET_ACCESS_KEY>',
  'host' => 's3.sub4.uni-freiburg.de',
  'aws_signature_version' => 2, # For creation of signed URLs. Set to 2 if provider does not support v4.
  'endpoint' => 'https://s3host1.uni-freiburg.de', # default: nil - Useful for S3 compliant services such as DigitalOcean Spaces
  'path_style' => false # (false) Use 'host/bucket_name/object' instead of 'bucket_name.host/object'
}
#gitlab_rails['object_store']['storage_options'] = {}
gitlab_rails['object_store']['proxy_download'] = false
gitlab_rails['object_store']['objects']['artifacts']['bucket'] = "bucket-gitlab-artifacts"
gitlab_rails['object_store']['objects']['dependency_proxy']['bucket'] = false
gitlab_rails['object_store']['objects']['external_diffs']['bucket'] = false
gitlab_rails['object_store']['objects']['lfs']['bucket'] = "bucket-gitlab-lfs"
gitlab_rails['object_store']['objects']['packages']['bucket'] = "bucket-gitlab-packages"
gitlab_rails['object_store']['objects']['pages']['bucket'] = "bucket-gitlab-pages"
gitlab_rails['object_store']['objects']['terraform_state']['bucket'] = false
gitlab_rails['object_store']['objects']['uploads']['bucket'] = "bucket-gitlab-uploads"
```

If you want to use different secrets for each bucket use s3 configuration in each service.
The following shows a s3 configuration for the Gitlab container registry.
This bucket can't be configured in the above example yet (see [Gitlab Doc](https://docs.gitlab.com/ee/administration/object_storage.html#storage-specific-configuration)) and has to be in the service section for container registry.

```yaml
registry['storage'] = {
  's3' => {
    'accesskey' => '<AWS_ACCESS_KEY_ID>',
    'secretkey' => '<AWS_SECRET_ACCESS_KEY>',
    'bucket' => 'bucket-gitlab-registry',
    'region' => '<AWS_DEFAULT_REGION>',
    'regionendpoint' => 's3.sub4.uni-freiburg.de',
    'path_style' => false # (false) Use 'host/bucket_name/object' instead of 'bucket_name.host/object'
  },
  'redirect' => {
    'disable' => false
  }
}
```

If you already have used file storage you can run the following to migrate to object storage.

Pages:
```bash
gitlab-rake gitlab:pages:deployments:migrate_to_object_storage
```

LSF, Artifacts, Uploads, Packages:
```bash
gitlab-rake gitlab:lfs:migrate
gitlab-rake gitlab:artifacts:migrate
gitlab-rake gitlab:uploads:migrate:all
gitlab-rake gitlab:packages:migrate
```

## Gitlab Runner

Gitlab runner are used for CI/CD and can be used for generating web sites for gitlab pages, etc.

This section focuses on a separate VM for a shared runner, which can be used by all users.
A runner can be one of these [executors](https://docs.gitlab.com/runner/executors/).
For simplicity this documentation focuses on the docker executor.

First create a new VM.
Use a flavor with a virtual disk of at least 20GB.
The docker containers can use a lot of space after some time.
If you want to build containers with CI you will need temporary some GB of space during CI/CD.

### Install Docker Engine

Gitlab Documentation: [Install Docker](https://docs.docker.com/engine/install/centos/)

Install dependencies and repo:
```bash
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Install Docker Engine and containderd:
```bash
yum install -y docker-ce docker-ce-cli containerd.io
```

Enable Docker:
```bash
systemctl enable --now docker.service
```


### Install Gitlab Runner

Gitlab Documentation: [Runner Installation](https://docs.gitlab.com/runner/install/linux-repository.html)

Bash:
```bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpcurl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
yum install -y gitlab-runner
```

### Register Shared Docker Runner

Gitlab Documentation: [Runner Registration](https://docs.gitlab.com/runner/register/)

For a shared runner, as administrator get the "registration token" by visiting the "Admin Area" and select Overview and Runners in the menu.

On your runner instance run `gitlab-runner register` and enter the following the following:
```bash
[root@gitlab-runner ~]# gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=11972 revision=de104fcd version=14.5.1
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
https://gitlab.sub1.uni-freiburg.de    # add your Gitlab instance
Enter the registration token:
<TOKEN>                                # copy token here
Enter a description for the runner:
[gitlab-runner.sub1.uni-freiburg.de]:  # add a name for your runner
Enter tags for the runner (comma-separated):
docker                                 # add tags, e.g. executor
Registering runner... succeeded                     runner=abc123
Enter an executor: virtualbox, docker-ssh, parallels, shell, ssh, docker+machine, docker-ssh+machine, kubernetes, custom, docker:
docker                                 #  add executor, here we use docker
Enter the default Docker image (for example, ruby:2.6):
ubuntu:latest                          # name of the default docker image
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

If you want to build and test Containers like Singularity, you need to enable privileged docker execution.
And while editing `/etc/gitlab-runner/config.toml` you can increase concurrency (`concurrent`) if needed.

```toml
/etc/gitlab-runner/config.toml:

  [runners.docker]
    privileged = true
```

Restart Runner:
```bash
gitlab-runner restart
```


## Gitlab Pages

Gitlab Documentation: [Gitlab Pages](https://docs.gitlab.com/ee/administration/pages/)

### Pages Running on the Same Server

If you want to run pages on the same instance as Gitlab, you should add a wildcard DNS `A record`:
```bash
*.sub1.uni-freiburg.de. 1800 IN A    <IP-ADDRESS-GITLAB>
```

If you are using `https://gitlab.sub1.uni-freiburg.de` for your Gitlab instance, the domain for pages should be `http://<PAGES-SUBDOMAIN>.sub1.uni-freiburg.de`:
```yaml
pages_external_url "http://pages.sub1.uni-freiburg.de" # not a subdomain of external_url
```

### Custom Domains

If you want to support custom domains configure a second IP.
You can use a configuration for same or separate Pages server.
If you use only one server you'll need to configure a second IP.

`A records`:
```bash
gitlab.sub1.uni-freiburg.de. 1800 IN A   <IP-ADDRESS-GITLAB>
*.sub1.uni-freiburg.de.      1800 IN A   <IP-ADDRESS-PAGES>
```

In your config add following lines:
```yaml
pages_external_url "http://pages.sub1.uni-freiburg.de" # not a subdomain of external_url
nginx['listen_addresses'] = ['<IP-ADDRESS-GITLAB>'] # The primary IP of the GitLab instance
gitlab_pages['enable'] = false
gitlab_pages['external_http'] = ['<IP-ADDRESS-PAGES>:80'] # The secondary IPs for the GitLab Pages daemon
```

Reconfigure Gitlab:
```bash
gitlab-ctl reconfigure
```

### Pages Running on a Separate Server

If you want to run a separate Pages instance on a different server than your Gitlab instance, please follow the next steps.

#### Changes on the Gitlab Server

First, create a backup of the secrets file on the GitLab server:
```bash
cp /etc/gitlab/gitlab-secrets.json /etc/gitlab/gitlab-secrets.json-$( date -I )
```

Add a wildcard DNS `A record` for the Gitlab Pages domain:
```bash
*.external1.uni-freiburg.de. 1800 IN A    <IP-ADDRESS-PAGES>
```

The domain for Pages should be `http://<PAGES-EXTERNAL>.external1.uni-freiburg.de`:

Change the following in your config:
```yaml
pages_external_url "http://pages.external1.uni-freiburg.de"
gitlab_pages['enable'] = false
gitlab_pages['external_http'] = ['<IP-ADDRESS-PAGES>']
pages_nginx['enable'] = false
```

Reconfigure Gitlab:
```bash
gitlab-ctl reconfigure
```

#### Changes on the Pages Server

First you need to install a second Gitlab instance and do some basic configuration from step [Configure Gitlab Instance](#Configure-Gitlab-Instance).
When the Pages server is ready, proceed with the configuration.

Create a backup of the secrets file on the GitLab server:
```bash
cp /etc/gitlab/gitlab-secrets.json /etc/gitlab/gitlab-secrets.json-$( date -I )

Copy `/etc/gitlab/gitlab-secrets.json` from the Gitlab server to the Pages server.

```

Add the following to `/etc/gitlab/gitlab.rb`:
```yaml
roles ['pages_role']
pages_external_url "http://pages.external1.uni-freiburg.de"
gitlab_pages['gitlab_server'] = 'https://gitlab.sub1.uni-freiburg.de'
```
Reconfigure Gitlab:
```bash
gitlab-ctl reconfigure
```

### Using Wildcard Certificates

If you want to use wildcard certificates for pages, copy your certificate and key to `/etc/gitlab/ssl`.

Example:
```bash
/etc/gitlab/ssl/pages.sub1.uni-freiburg.de.crt
/etc/gitlab/ssl/pages.sub1.uni-freiburg.de.io.key
```

If you used a different name, change the path in the config.
`/etc/gitlab/gitlab.rb`:
```yaml
pages_nginx['ssl_certificate'] = "/etc/gitlab/ssl/pages-nginx.crt"
pages_nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/pages-nginx.key"
```

To use `HTTPS` add this information to your config.
`/etc/gitlab/gitlab.rb`:
```yaml
pages_external_url "https://pages.sub1.uni-freiburg.de" # not a subdomain of external_url
pages_nginx['redirect_http_to_https'] = true
```

If you support custom domains add this line to your config:
```yaml
gitlab_pages['external_https'] = ['<IP-ADDRESS-PAGES>:443', '[2001:db8::2]:443'] # The secondary IPs for the GitLab Pages daemon
```


### Pages Access Control

For access control of your Pages, add the following line to your config:
```yaml
gitlab_pages['access_control'] = true
```
