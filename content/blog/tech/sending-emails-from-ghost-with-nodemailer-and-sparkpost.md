---
title: "Sending emails from Ghost with Nodemailer and SparkPost"
description: "How to use Nodemailer and SparkPost to send transactional emails from the Ghost blogging platform"
date: 2016-06-14T01:08:22+02:00
blog: ["tech"]
categories: ["tech"]
tags: ["node.js", "ghost"]
aliases: ["/tech/2016/06/14/sending-emails-from-ghost-with-nodemailer-and-sparkpost/", "/tech/2016/06/14/sending-emails-from-ghost-with-nodemailer-and-sparkpost"]
---

While I personally prefer Jekyll to Ghost in the battle of minimalistic CMS platforms, I recently set up a [Ghost](https://ghost.org/) blog for work and connected the built-in [Nodemailer](https://github.com/nodemailer/nodemailer) email module with [SparkPost](https://www.sparkpost.com), an email service. Here's how:

1. Set up a SparkPost account and verify your sending domain.
2. Go to SparkPost **Accounts** --> **SMTP Relay** and copy the host, port and username.
3. Under **Accounts** --> **API Keys**, select the **Send via SMTP** permission and generate a new API key. The generated API key will be your SMTP password.
4. Open up your Ghost blog's config.js, and modify the production section.

```javascript
  production: {
    url: 'https://example.com/blog',
    mail: {
      from: '"Example.com Blog" <no-reply@example.com>',
      transport: 'SMTP',
      options: {
        host: 'smtp.sparkpostmail.com',
        port: 587,
        auth: {
          user: 'SPARKPOST_SMTP_USERNAME',
          pass: 'SPARKPOST_SMTP_API_KEY'
        }
      }
    },
    database: {
    ....
    }
  }
```

You can test your configuration by inviting someone to your Ghost installation. The invitee should receive an email from *Example.com blog*.
