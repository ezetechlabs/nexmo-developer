---
title: How to Generate Test Credentials
products: client-sdk
description: This tutorial shows you how to create a Nexmo application, users and tokens.
languages:
    - Cross platform
---

# How to Generate Test Credentials

In order to use the Nexmo Client SDK, there are three things you need to set up before getting started:

* [Nexmo Application](/conversation/concepts/application) - an Application which contains configuration for the app that you are building.

* [Users](/conversation/concepts/user) - Users who are associated with the Nexmo Application. It is expected that Users will have a one-to-one mapping with your own authentication system.

* [JSON Web Tokens, JWTs](https://jwt.io/) - Nexmo Client SDK uses JWTs for authentication. In order for a user to log in and use the SDK functionality, you need to provide a JWT per user. JWTs contain all the information the Nexmo platform needs to authenticate requests, as well as information such as the associated Applications, Users and permissions.

All of these may be created by your backend. If you wish to get started and experience using the SDK without any implementation of your backend, this tutorial will show you how to do so, using the Nexmo CLI.

## Prerequisites

Make sure you have the following:

* A Nexmo account - [signup](https://dashboard.nexmo.com)
* [Node.JS](https://nodejs.org/en/download/) and NPM installed
* Install the Nexmo CLI.

To install the Nexmo CLI, run the following in a terminal:

```bash
$ npm install -g nexmo-cli@beta
```

Set up the Nexmo CLI to use your Nexmo API Key and API Secret. You can get these from the [settings page](https://dashboard.nexmo.com/settings) in the Nexmo Dashboard.

```bash
nexmo setup api_key api_secret
```

## Create a Nexmo Application

Create an application within the Nexmo platform.

Run the following command:

```bash
nexmo app:create "My Nexmo App" http://example.com/answer http://example.com/event --type=rtc --keyfile=private.key
```

The output will be similar to:

```bash
Application created: aaaaaaaa-bbbb-cccc-dddd-0123456789ab
No existing config found. Writing to new file.
Credentials written to /path/to/your/local/folder/.nexmo-app
Private Key saved to: private.key
```

On the first output line is the Application ID. Take a note of it. It will be later referred to as `MY_APP_ID`.

In addition, a private key is created. It is used to generate JWTs that are used to authenticate your interactions with Nexmo.

## Create a User

Create a user who will log in to Nexmo Client and participate in the SDK functionality: Conversations, Calls and so on.

Replace `MY_USER_NAME` with your desired user name, and run the following command on a terminal:

```bash
nexmo user:create name="MY_USER_NAME"
```

The output with the user ID, is similar to:

```sh
User created: USR-aaaaaaaa-bbbb-cccc-dddd-0123456789ab
```

Take a note of the `id` attribute as this is the unique identifier for the user that has been created. It will be  referred to as `MY_USER_ID` in the next step.

## Generate a User JWT

Generate a JWT for the user. Remember to replace `MY_APP_ID` and `MY_USER_ID` values in the command:

```bash
nexmo jwt:generate ./private.key sub=MY_USER_ID exp=$(($(date +%s)+86400)) acl='/**' application_id=MY_APP_ID
```

> **NOTE**: The above command sets the expiry of the JWT to one day from now, which is the maximum amount of time. You may change the expiration to a shortened amount of time, or re-grenerate a JWT for the user after the current JWT has expired.

You can read more about the JWT and ACL [in this topic](/client-sdk/concepts/jwt-acl).