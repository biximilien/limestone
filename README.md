# SaaS Template 💸

TODO: Summary of diverging settings

This fork of [Limestone](https://github.com/archonic/limestone) by [Archonic](https://github.com/archonic) is a boilerplate SaaS app template built with Rails 6 on Ruby 2.7.1 and has diverging opinionated defaults on the Gemset and Deployment philosophy.

Summary of diverging changes from upstream :
* docker
* ci
* Boostrap CSS framework is replaced with Bulma
* ...

Limestone assumes you want each user to pay for access to your SaaS. If instead you want users to belong to accounts and have billing scoped to accounts, try [Limestone Accounts](https://github.com/archonic/limestone-accounts).

## Versions
Versioning in this repo in intended to maintain and modernize the boilerplate. New versions are not intended to update existing forks, although looking through the commits serve as a good upgrade resource.

v0.1 is Rails 5.2  
v0.2 is Rails 6  
v0.3 introduces the [Pay gem](https://github.com/pay-rails/pay)  

See more in the [changelog](https://github.com/archonic/limestone/blob/master/CHANGELOG.md).

## The Stack
The [gemset](https://github.com/archonic/limestone/blob/master/Gemfile) has been chosen to be modern, performant, and take care of a number of business concerns common to SaaS.

## Features
* Free trial begins upon registration without credit card. Number of days is configurable with ENV var.
* Subscription management. Card update form, switch plan form and cancel account button.
* [Devise confirmable](https://github.com/heartcombo/devise/wiki/How-To:-Add-:confirmable-to-Users) installed and configured.
* Emails for welcome, receipt, refund, subscription renewing and payment action required.
* [letter_opener](https://github.com/ryanb/letter_opener) and [letter_opener_web](https://github.com/fgrehm/letter_opener_web) installed and configured. Visit /admin/letter_opener in development to see emails sent.
* Mail sends through Sidekiq with deliver_later for production. [Sendgrid](https://sendgrid.com/) is configured and ready to use once your API keys are set in ENV.
* Direct cloud uploading with [ActiveStorage](https://edgeguides.rubyonrails.org/active_storage_overview.html). Lazy transform for resizing. Demonstrated with user avatars.
* Icon helper for user avatars with fallback to user initials.
* Icon helper for [Font Awesome 4.7](https://fontawesome.com/v4.7.0/icons/) icons.
* Administrate dashboard lets you CRUD records. Easy to add more models and customize as you like. Visit /admin/.
* Impersonate users through Administrate dashboard.
* Pretty modals using Bootstrap integrated into rails_ujs data-confirm. Demonstrated with cancel account button.
* Banner with a link to billing page users that are past due.
* Opinionated search integration using Elasticsearch via Searchkick. Gem is in place but integration is up to you.
* Feature control using the [Flipper](https://github.com/jnunemaker/flipper) gem. Demonstrated with the `public_registration` feature.
* 84% RSpec test coverage.

## Notes
* RSpec controller tests have been omitted in favour of requests tests.

## Pre-requisites

### Development
* A [Stripe](https://dashboard.stripe.com/register) account and a [Stripe API Key](https://stripe.com/docs/keys).

### Test
* **NOTE** Limestone expects your product prices to have trial days > 0. If you don't create a trail, testing will get the error `Pay::Error: This customer has no attached payment source or default payment method.`.

### Production
* A cloud storage account [compatible with ActiveStorage](https://edgeguides.rubyonrails.org/active_storage_overview.html#setup).

## Getting Started
1. Clone this repository at the most recent tag and `cd` into it:
    ```
    git clone -b 'v0.3' --single-branch --depth 1 https://github.com/archonic/limestone.git
    cd limestone
    ```

2. Make a copy of `.env-example` named `.env`:
    ```
    cp .env-example .env
    ```

3. Update the `.env` file - running the project requires you change the following:
    - `STRIPE_API_KEY`
    - `STRIPE_PUBLISHABLE_KEY`
    - `STRIPE_SIGNING_SECRET` (This can be something random)

    You probably want to update the `ADMIN_*` environment variables. If you want a different `COMPOSE_PROJECT_NAME` and database name, now is the best time to do that.

6. Visit [http://localhost:3000](http://localhost:3000) and rejoice :tada: You can login using the Admin user defined in `.env`. Keep in mind your admin doesn't have active billing. Enter a [test card](https://stripe.com/docs/testing#cards) when prompted or by visiting /subscribe.

### Note About Flipper / Public User Registration
1. The [Flipper gem](https://github.com/jnunemaker/flipper) controls feature flagging and provides a UI. Visit the `/admin/flipper`.
2. The feature called `public_registration` has been created for you (during seeding). You can enable/disable this to control user registration :clap:
