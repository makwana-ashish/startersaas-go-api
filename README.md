# Installation
Make sure you have MongoDB (4+) installed and running.

Then make sure you have Go installed ([download](https://golang.org/dl/)). Version `1.14` or higher is required.

Install all dependencies by running 

```bash
go get
```

Copy `.env.example` into `.env` and `stripe.conf.json.example` into `stripe.conf.json`.

Finally, run the APIs by typing:

```bash
go run main.go
```


# Stripe setup

Configure your stripe webhooks by setting as "Endpoint URL" :

```
https://<my_startersaas_api_domain>/api/v1/stripe/webhook
```

and events below:

```
invoice.payment_succeeded
invoice.payment_failed
customer.subscription.created
customer.subscription.updated
```

Configure Stripe to retry failed payments for X days, and then cancel the subscription. Remember this value, it will be used in the ```.env``` file.

# Configuring .env

Below the meaning of every environment variable you can setup.


`PORT=":3000"`  the API server port number

`CORS_SITES="*"` allow only a specific domain to perform Ajax requests

`DATABASE="startersaas-db"` the MongoDB database 

`DATABASE_URI="mongodb://localhost:27017"` the MongoDB connection string

`JWT_SECRET="aaabbbccc"` keep this value secrect and very long and random

`JWT_EXPIRE="1d"` # how long the JWT token last

`FRONTEND_LOGIN_URL="http://localhost:5000/auth/login"` raplace http://localhost:5000 with the real production host

`FRONTEND_FORGOT_URL="http://localhost:5000/auth/reset-password"` raplace http://localhost:5000 with the real production host

`MAILER="localhost:1025"` the SMTP mailer connection string

`DEFAULT_EMAIL_FROM="noreply@startersaas.com"` send every email from this address

`LOCALE="it"` the deafalt locale for registered users

`STRIPE_SECRET_KEY="sk_test_xyz"` the Stripe secret key

`FATTURA24_KEY="XYZ"` the Fattura 24 secret key (Italian market only)

`FATTURA24_URL="https://www.app.fattura24.com/api/v0.3/SaveDocument"` do not change this value

`TRIAL_DAYS=15` how many days a new user can work without subscribing

`PAYMENT_FAILED_RETRY_DAYS=7` how many days a user can work after the first failed payment (and before Stripe cancel the subscription)

`NOTIFIED_ADMIN_EMAIL="info@startersaas.com"` we notify admins when some events occur, like a new subscription, a failed payment and so on


# Configuring stripe.conf.json

In this file you have to add the stripe API public key, in the ```publicKey```field

Then for every product you want to sell, copy it's price_id (usually starts with price_xxx) and paste it in the id key.

```
{
  "id": "price_xyz",
  "title": "Starter - Piano Mensile",
  "price": 199,
  "currency": "EUR",
  "features": [
    "Piano editoriale mensile",
    "1 post Facebook/settimana",
    "1 post Instagram/settimana",
    "1 Facebook story/settimana",
    "1 Instagram story/settimana",
    "1 articolo il blog ottimizzato SEO",
    "-",
    "-",
    "-",
    "-"
  ],
  "monthly": true
}
```

Then sets its title, its price (in cents, the same you have configured in Stripe) and the list of features you want to show in the frontend pricing table. Finally set ```"monthly":true``` if your plan is billed on monthly basis, otherwise we consider it billed yearly.


# Features

### API and Frontend

* user registration of account with subdomain, email and password
* user email activation with 6 characters code and account creation
* resend activation code if not received
* user password reset through code sent by email
* user login
* user logout
* user change password once logged in
* account trial period
* edit of account billing informations
* subscription creation
* plan change
* add new credit card
* subscription cancel
* 3D Secure ready payments

### API only

* account's users list (by admins only)
* account's user create (by admins only)
* account's user update (by admins only)
* stripe webhooks handling
* events notifications by email:
  - new user subscribed
  - succesful payments
  - failed payments
* daily notifications by email:
  - expiring trials
  - failed payments
  - account suspension due to failed payments

