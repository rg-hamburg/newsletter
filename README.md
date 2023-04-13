# 📬 RG Hamburg BMX Newsletter

These are the sources for the RG Hamburg BMX Newsletter. The Newsletter
is built using [MJML](https://documentation.mjml.io/) and is based on the
[Recast Template](https://mjml.io/try-it-live/templates/recast).

## Editing the newsletter

...

## Sending the newsletter

First, render the issue into HTML.

```
ISSUE=01-april-2023
./node_modules/.bin/mjml issues/${ISSUE}.mjml \
    --config.beautify true --config.minify true \
    --output docs/${ISSUE}.html
```

Then, we're sending out the issue

https://dev.mailjet.com/email/guides/send-api-v31/#send-a-basic-email

```bash
MJ_APIKEY_PUBLIC=
MJ_APIKEY_PRIVATE=
ISSUE_CONTENT=$(cat docs/${ISSUE}.html)
jq -n --arg ISSUE_CONTENT ${ISSUE_CONTENT} -f mailjet-api-payload-test.json > ${ISSUE}.json
curl -s \
	-X POST \
	--user "${MJ_APIKEY_PUBLIC}:${MJ_APIKEY_PRIVATE}" \
	https://api.mailjet.com/v3.1/send \
	-H 'Content-Type: application/json' \
	-d @${ISSUE}.json
```


## Setting up the project

For editing, either use the VSCode MJML extension or install
the desktop application (`brew install --cask mjml`).

```
npm install mjml
```
