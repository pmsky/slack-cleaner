# slack-cleaner

Bulk delete messages and files on Slack.

this is a fork of https://github.com/kfei/slack-cleaner

## Install

Install from Pip:

```bash
pip install -e git+https://github.com/sgratzl/slack-cleaner.git#egg=slack-cleaner
```

If you prefer Docker, there is a pre-built Docker image as well:

```bash
docker pull sgratzl/slack-cleaner
```

Just use `docker run -it --rm sgratzl/slack-cleaner -c "slack-cleaner ..."` for each command or jump into a shell using `docker run -it --rm sgratzl/slack-cleaner`.

## Arguments
```
usage: slack-cleaner [-h] --token TOKEN [--log] [--quiet] [--rate RATE]
                     [--as_user] [--message | --file | --info] [--regex]
                     [--channel CHANNEL] [--direct DIRECT] [--group GROUP]
                     [--mpdirect MPDIRECT] [--user USER] [--botname BOTNAME]
                     [--bot] [--keeppinned] [--after AFTER] [--before BEFORE]
                     [--types TYPES] [--pattern PATTERN] [--perform]

optional arguments:
  -h, --help           show this help message and exit
  --token TOKEN        Slack API token (https://api.slack.com/web)
  --log                Create a log file in the current directory
  --quiet              Run quietly, does not log messages deleted
  --rate RATE          Delay between API calls (in seconds)
  --as_user            Pass true to delete the message as the authed user. Bot
                       users in this context are considered authed users.
  --message            Delete messages
  --file               Delete files
  --info               Show information
  --regex              Interpret channel, direct, group, and mpdirect as regex
  --channel CHANNEL    Channel name's, e.g., general
  --direct DIRECT      Direct message's name, e.g., sherry
  --group GROUP        Private group's name
  --mpdirect MPDIRECT  Multiparty direct message's name, e.g.,
                       sherry,james,johndoe
  --user USER          Delete messages/files from certain user
  --botname BOTNAME    Delete messages/files from certain bots
  --bot                Delete messages from bots
  --keeppinned         exclude pinned messages from deletion
  --after AFTER        Delete messages/files newer than this time (YYYYMMDD)
  --before BEFORE      Delete messages/files older than this time (YYYYMMDD)
  --types TYPES        Delete files of a certain type, e.g., posts,pdfs
  --pattern PATTERN    Delete messages with specified pattern (regex)
  --perform            Perform the task
```

## Minimal Slack permission scopes required

- `channels:history`
- `channels:read`
- `chat:write:bot`
- `users:read`


## Usage

```bash
# Delete all messages from a channel
slack-cleaner --token <TOKEN> --message --channel general --user "*"

# Delete all messages from a private group aka private channel
slack-cleaner --token <TOKEN> --message --group hr --user "*"

# Delete all messages from a direct message channel
slack-cleaner --token <TOKEN> --message --direct sherry --user johndoe

# Delete all messages from a multiparty direct message channel. Note that the
# list of usernames must contains yourself
slack-cleaner --token <TOKEN> --message --mpdirect sherry,james,johndoe --user "*"

# Delete all messages from certain user
slack-cleaner --token <TOKEN> --message --channel gossip --user johndoe

# Delete all messages from bots (especially flooding CI updates)
slack-cleaner --token <TOKEN> --message --channel auto-build --bot

# Delete all messages older than 2015/09/19
slack-cleaner --token <TOKEN> --message --channel general --user "*" --before 20150919

# Delete all files
slack-cleaner --token <TOKEN> --file --user "*"

# Delete all files from certain user
slack-cleaner --token <TOKEN> --file --user johndoe

# Delete all snippets and images
slack-cleaner --token <TOKEN> --file --types snippets,images

# Show information about users, channels:
slack-cleaner --token <TOKEN> --info

# TODO add pattern example, add keep_pinned example, add quiet

# Always have a look at help message
slack-cleaner --help

```

## Tokens

You will need to generate a Slack legacy token to use slack-cleaner. You can generate a token [here](https://api.slack.com/custom-integrations/legacy-tokens):

[https://api.slack.com/custom-integrations/legacy-tokens](https://api.slack.com/custom-integrations/legacy-tokens)

## Tips

After the task, a backup file `slack-cleaner.<timestamp>.log` will be created in current directory if `--log` is supplied.

If any API problem occurred, try `--rate=<delay-in-seconds>` to reduce the API call rate (which by default is unlimited).

If you see the following warning from `urllib3`, consider to install missing
packages: `pip install --upgrade requests[security]` or just upgrade your Python to 2.7.9.

```
InsecurePlatformWarning: A true SSLContext object is not available.
          This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail.
          For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.
```

## Credits

**To all the people who can only afford a free plan. :cry:**
