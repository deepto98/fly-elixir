# Solution
## Understanding the problem
### Invoicing & Billing
* Handles billing for a bunch of different things.
* Invoices are a combination of metered product usage, one-off line items, and recurring subscription fees.
* Models global account data with concepts like Organizations and Users.
### Current process for billing devs:
1. Sync usage data from a variety of sources to Stripe.
2. Then Stripe generates an invoice based on Stripe's knowledge of our products and pricing.
### Challenge : 
* We bill for a whole lot of things in tiny increments, so we need to sync usage data to Stripe all the time. We sometimes fail to sync at all, which means we can't tell our users how much they owe.
### My task :
* An implementation that does not aggressively sync to Stripe
### Provided Code
Approach suggested:
* we could compile our own usage data and generate invoices in our app, instead of syncing usage data to Stripe.
* Once an invoice is generated, we can sync the invoice, rather than the usage, to Stripe and use that to bill our customers.
* `lib/fly/billing/invoice.ex` : top-level invoice which we'll use to bill developers monthly.
* `lib/fly/billing/invoice_item.ex` : a line item that belongs to an invoice. It can represent metered product usage, a one-off fee, or a recurring subscription fee.
* `lib/fly/organizations/organization.ex` : entity that gets invoiced.
* `lib/fly/billing.ex`
* `lib/fly/stripe.ex` : Mock strip library
* `priv/repo/seeds.exs` : script for seeding data in our database
### Setup
1. Install asdf - https://asdf-vm.com/guide/getting-started.html
2. Install asdf plugins and project dependencies:
```    
    asdf plugin add erlang https://github.com/asdf-vm/asdf-erlang.git
    asdf plugin add elixir https://github.com/asdf-vm/asdf-elixir.git
    asdf install
```
3. Run postgres :
`docker run -d -p 5432:5432 -e POSTGRES_HOST_AUTH_METHOD=trust postgres`
4. Run app:
   1. mix setup
   2. mix phx.server
5. Seed db: mix run priv/repo/seeds.exs

### Stuff I'm going through
1. Todo : Understand context and schema. From the code, I think schema represents the data model, similar to a struct, and context has the functions implemented using objects made with the schema. The folder `lib/fly` itself has the context files - `organization.ex`,`billing.ex`,etc and it has folders to store the schema - `lib/fly/organizations/organization.ex` i.e the organization folder contains the schema for organization 
2. Elixir Contexts and Ecto Schema : https://steemit.com/utopian-io/@tensor/intro-to-elixir-ecto-schemas-and-phoenix-contexts#:~:text=Contexts%20are%20modules%20that%20expose,case%20we%20are%20using%20PostgreSQL).
3. Mix : Mix is a build tool that ships with Elixir that provides tasks for creating, compiling, testing your application, managing its dependencies and much more. This is probably similar to npm/yarn

