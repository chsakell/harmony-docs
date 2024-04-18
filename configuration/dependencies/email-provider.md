# 📧 Email provider

<mark style="color:blue;">**Harmony.Notifications**</mark> currently supports Gmail & [Brevo](https://www.brevo.com/products/transactional-email/) email service provider integration.

### Gmail setup

For the Gmail setup set the **GmailSettings** property in the _**appsettings.json**_ file in <mark style="color:blue;">**Harmony.Notifications**</mark>.

```json
  "GmailSettings": {
    "From": "",
    "SmtpServer": "smtp.gmail.com",
    "Port": 465,
    "UserName": "",
    "Password": "",
    "DisplayName": ""
  },
```

Each email service provider, e.g Gmail, Outlook, etc.. has its own settings so please advice and setup accordingly. Keep in mind that some providers, for example Gmail require that you have setup **Two Factor Authentication** to your account and create a specific **App Password** for your application. Following is an example of a Gmail account, assumming the email address is harmony@gmail.com:

```json
  "GmailSettings": {
    "From": "harmony@gmail.com",
    "SmtpServer": "smtp.gmail.com",
    "Port": 465,
    "UserName": "harmony",
    "Password": "xxxx yyyy zzzz wwww",
    "DisplayName": "Harmony"
  },
```

{% hint style="warning" %}
The password **xxxx yyyy zzzz wwww** isn't the actual password for signing with the harmony username. It's a new **app password** created specifically on its google account to be used for the smtp settings. After adding **2FA authentication** on your Gmail account, search for **App passwords** and create a password for your app. Then paste that password in the corresponding property in appsettings.json file.
{% endhint %}

Last thing you need to do is to register the Gmail service provider in code. Ensure you don't have any other registrations for the <mark style="color:blue;">IEmailService</mark> interface.

```csharp
builder.Services.AddSingleton<IEmailService, GmailEmailService>();
```

### Brevo setup

Brevo setup is much easier to configure. Follow the next steps to create an API key:

1. Register an [account](https://onboarding.brevo.com/account/register).
2. Navigate to [/settings/key/api/](https://app.brevo.com/settings/keys/api) and click **Generate new API key**.
3.  Fill the **BrevoSettings** property in the appsettings.json file:

    ```
      "BrevoSettings": {
        "ApiKey": "paste-your-key-here",
        "SenderEmail": "harmony@no-reply.com",
        "SenderName" : "Harmony"
      },
    ```

Last thing you need to do is to register the Brevo service provider in code. Ensure you don't have any other registrations for the <mark style="color:blue;">IEmailService</mark> interface.

```csharp
builder.Services.AddSingleton<IEmailService, BrevoEmailService>();
```

#### Read next - Deploy Harmony

{% content-ref url="../deployment.md" %}
[deployment.md](../deployment.md)
{% endcontent-ref %}
