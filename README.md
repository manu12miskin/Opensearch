# Opensearch

## Opensearch installation

### Step 1 – Add OpenSearch Repository

1. First, install the required dependencies.

```
apt -y install curl lsb-release gnupg2 ca-certificates
```

2. Next, import the GPG key to sign the OpenSearch packages.

```
curl -fsSL https://artifacts.opensearch.org/publickeys/opensearch.pgp| gpg --dearmor -o /etc/apt/trusted.gpg.d/opensearch.gpg
```

3. Next, add the OpenSearch repository to your system.
```
echo "deb https://artifacts.opensearch.org/releases/bundle/opensearch/2.x/apt stable main" | tee /etc/apt/sources.list.d/opensearch-2.x.list
```

4. Update the package lists to reflect the changes you made to the repository sources
```
apt update -y
```

### Step 2 – Install and Configure OpenSearch

1. To install OpenSearch with an initial admin password set, you can use environment variables during the installation process. Here’s the command to install OpenSearch with the initial admin password set.
```
env OPENSEARCH_INITIAL_ADMIN_PASSWORD=opensearch@admin apt install opensearch
```
  
   This command sets the initial admin password for OpenSearch to “opensearch@admin” during the installation process. 
Replace “opensearch@admin” with your desired password.

5. Edit the OpenSearch default configuration file.
```
nano /etc/opensearch/opensearch.yml
```

Change the default values as per your requirements:

```
network.host: 0.0.0.0

discovery.type: single-node

plugins.security.disabled: false
plugins.security.ssl.http.enabled: false
```

Save and close the file, then reload the systemd daemon and restart opensearch.

```
systemctl daemon-reload
systemctl restart opensearch
```

3. Check the opensearch status
```
systemctl status opensearch
```
### Step 3 – Install OpenSearch Dashboard

The OpenSearch Dashboard, also known as the OpenSearch Kibana, is a web-based interface that allows users to interact with OpenSearch clusters, visualize data, and perform various data analysis tasks. It provides a user-friendly environment for exploring and analyzing data stored in OpenSearch.

1. Add the OpenSearch Dashboard repository.
```
echo "deb https://artifacts.opensearch.org/releases/bundle/opensearch-dashboards/2.x/apt stable main" | tee -a /etc/apt/sources.list.d/opensearch-2.x.list
```

2. Update the repository index and install the OpenSearch Dashboard.
```
apt update
apt install opensearch-dashboards
```

3. Edit the OpenSearch Dashboard configuration file.
```
nano /etc/opensearch-dashboards/opensearch_dashboards.yml
```

Change the following settings:

```
server.host: "0.0.0.0"

server.ssl.enabled: false

opensearch.hosts: ["http://localhost:9200"]
```

Save and close the file, then restart the OpenSearch Dashboard service.
```
systemctl restart opensearch-dashboards
```

4. Check the OpenSearch Dashboard service status.
```
systemctl status opensearch-dashboards
```

### Step 4 – Access OpenSearch Dashboard

By default, OpenSearch Dashboard listens on port 5601. You can access it using the URL http://your-server-ip:5601. You will see the OpenSearch Dashboard login page.
Provide the OpenSearch Dashboard admin username as a "admin" and password as "opensearch@admin" or default username as a kibanauser and password as kibanauser, then click on Login. 
