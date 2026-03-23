# Extended Guide

## Environment variables

Environment variables are like a configuration file. They are stored in a file called `.env` in the same directory as the scripts. You have to create this file yourself (an example file is provided in the repository). Below is a list of the most important environment variables and their descriptions.

### Obtaining your Cloudflare credentials

#### `CLOUDFLARE_ACCOUNT_ID`

You can find your Cloudflare account ID in the [Cloudflare dashboard](https://dash.cloudflare.com/). Click on any domain you own, then on the "Overview" tab. Scroll down to the bottom of the page and you will see your account ID on the right side.

Alternatively, you can copy the account ID from the URL of any page in the Cloudflare dashboard. The URL will look like this:

```text
https://dash.cloudflare.com/1234567890abcdef1234567890abcdef
```

In this example, `1234567890abcdef1234567890abcdef` is the account ID.

#### `CLOUDFLARE_API_TOKEN`

Cloudflare API Token can be created in your [Cloudflare profile](https://dash.cloudflare.com/profile/api-tokens):

1. Click "Create Token" and click "Get Started" in the "Create Custom Token" row.
2. Enter any name for your token
3. Scope the token to the specific account used by this project.
4. Add the minimal permissions required by this repository:
	- `Zero Trust Gateway Lists: Read`
	- `Zero Trust Gateway Lists: Write`
	- `Zero Trust Gateway Rules: Read`
	- `Zero Trust Gateway Rules: Write`
5. If your Cloudflare UI only exposes broader "Zero Trust" scopes, choose the smallest available read/edit scopes for that account.
6. Click "Continue to summary" and click "Create Token"
7. You will see the created API Token

![Creating API Token](.github/images/create_api_token.png)

#### `CLOUDFLARE_LIST_ITEM_LIMIT`

This value controls how many domains CGPS puts into each list chunk when creating/updating lists. It is also used to calculate how many lists CGPS will create. Default is 300,000.

This is separate from the internal API pagination size used by the script to read list items (`LIST_ITEM_PAGE_SIZE = 1000`).

### Other

#### `DRY_RUN`

Processes block/allow lists without actually adding/removing domains from Cloudflare. Avoid using this option unless you know what you are doing.

## Example usage

These commands should be run in a terminal.

```bash
# Assumes you have already cloned the repository and installed dependencies such as git and node.js.

# 1. Clone the repository
git clone https://github.com/mrrfv/cloudflare-gateway-pihole-scripts
cd cloudflare-gateway-pihole-scripts # change the current directory to the cloned repository
# 2. Run npm install to install dependencies.
npm install
# 3.1. Copy the example.env file to .env
# Files starting with a dot are hidden by default on Linux and macOS.
cp example.env .env
# 3.2. Edit the .env file and add your Cloudflare credentials.
# (This command varies depending on your system and preferred text editor.)
nano .env
# 4. If this is a subsequent run, execute node cf_gateway_rule_delete.js and node cf_list_delete.js (in order) to delete old data.
# Not needed if setting up for the first time.
node cf_gateway_rule_delete.js
node cf_list_delete.js
# 5. Download the default filter lists.
# If you want to use your own lists, put your blocked and allowed domains in files called "blocklist.txt" and "allowlist.txt" respectively.
node download_lists.js
# 6. Send the filter lists to Cloudflare.
node cf_list_create.js
# 7. Add the Cloudflare firewall rule.
node cf_gateway_rule_create.js
# Set up your DNS settings to use Cloudflare Gateway if you haven't already.
# 8. Profit!
ping google-analytics.com
```
