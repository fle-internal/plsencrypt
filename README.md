# plsencrypt

## Motivation

Ever wondered how much of a hassle it is to retrieve your certificates from Let's Encrypt? How there's so many steps you have to do when you don't have access to the server your domain is pointing to?

## Well wonder no more!

This repo comes with an executable called `plsencrypt`, which initiates the `letsencrypt` ACME protocol, adds the challenge response to your code, deploys your code, then continues with the ACME protocol to verify your key.

## Steps:

First, make sure you have your code with `app.yaml` in it. Insert this snippet in your `app.yaml`:

```
handlers:
    # URL for verifying letsencrypt.org challenge response questions
  - url: /\.well-known/acme-challenge/(.+)
    upload: challenges/(.*)
    static_files: challenges/\1
    mime_type: text/plain
    application_readable: True
    secure: optional
```

That's needed so Google will serve the challenge response text in the URL that `letsencrypt` expects.

After that, run `./plsencrypt` with the following flags:

```
./plsencrypt --appid=<google cloud app id> --domains=<domain you want to register, comma separated if multiple> --app-path=<path to app.yaml> --email=<domain owner email>
```

After some time, it should print out your certificates to the console, which you can copy to Google App Engine's configuration page.
