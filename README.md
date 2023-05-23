# 📬 RG Hamburg BMX Newsletter

These are the sources for the RG Hamburg BMX Newsletter. The Newsletter
is built using [MJML](https://documentation.mjml.io/) and is based on the
[Recast Template](https://mjml.io/try-it-live/templates/recast).

## Editing the newsletter

...

## Sending the newsletter

First, render the issue into HTML into the docs folder.
This will make it linkable under `https://newsletter.rg-hamburg.de/${ISSUE}.html`

```bash
export ISSUE=05-mai-2023
export ISSUE_NAME="#05 Mai 2023"

# render MJML to HTML
./node_modules/.bin/mjml issues/${ISSUE}.mjml \
    --config.beautify true --config.minify true \
    --output docs/${ISSUE}.html

# replace placeholders!
envsubst < docs/${ISSUE}.html | sponge docs/${ISSUE}.html
```

Then, we're sending out the issue via [MailJet](https://dev.mailjet.com/email/guides/send-api-v31/#send-a-basic-email).

```bash
# configure issue
MJ_APIKEY_PUBLIC=
MJ_APIKEY_PRIVATE=
issue_content=$(cat docs/${ISSUE}.html)
receipient="bmx-alle@..."

# create MailJet payload
jq -n --arg content ${issue_content} \
	--arg receipient ${receipient} \
	--arg issue ${ISSUE_NAME} \
	-f mailjet-api-payload.json > ${ISSUE}.json

# send email
curl -s \
	-X POST \
	--user "${MJ_APIKEY_PUBLIC}:${MJ_APIKEY_PRIVATE}" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d @${ISSUE}.json

# clean up
rm -f ${ISSUE}.json
```


## Setting up the project

For editing, either use the VSCode MJML extension or install
the desktop application (`brew install --cask mjml`). For
transforming MJML into HTML, you need to:

```
brew install jq
npm install mjml
```
