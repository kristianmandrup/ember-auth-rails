# ember-auth-rails

`ember-auth` provides token authentication support to
[ember.js](http://emberjs.com/).

## Installation

Add this line to your application's Gemfile:

    gem 'ember-auth-rails'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install ember-auth-rails

## Ember versions supported

Should support `Ember 1.0.pre1` and later.

## Ember rails

Requires and supports `ember-rails 0.10` and later.

## Usage

See [README](https://github.com/heartsentwined/ember-auth) at the main
`ember-auth` site.

## API

This gem creates the top level namespace `Auth`.

### Auth

The `Auth` namespace stores:

* authToken
* currentUserId
* prevRoute

For the current user.

It has these methods:

* signIn
* signOut
* resolveUrl(path)
* resolveRedirectRoute(type)

For `signIn` success:

```
success: function(json, status, jqxhr) {
  _this.set('authToken', json[Auth.Config.get('tokenKey')]);
  _this.set('currentUserId', json[Auth.Config.get('idKey')]);
  _this.set('jqxhr', jqxhr);
```

So it will try to fetch the id of the `currentUserId` from a returned json hash looking up the value for the key `Auth.Config.get('idKey')`. Similaraly for `authToken`.

For `signOut` success, it clears these same instance variables.

### Auth.Config

Auth.Config is used to configure Authentication details. These settings are fx used by methods such as `signIn` 

```
Auth.Config = Em.Object.create({
  tokenCreateUrl: null,
  tokenDestroyUrl: null,
  tokenKey: null,
  idKey: null,
  baseUrl: null,
  signInRoute: null,
  signOutRoute: null,
  authRedirect: false,
  smartSignInRedirect: false,
  smartSignOutRedirect: false,
  signInRedirectFallbackRoute: 'index',
  signOutRedirectFallbackRoute: 'index',
  rememberMe: false,
  rememberTokenKey: null,
  rememberPeriod: 14
});
```

## Auth.Route

Includes a `redirect` method which is used to redirect to a login route unless the user has previously been authenticated.

## Auth.SignInController

The method `registerRedirect` registers an observer on `authToken`. When it is changed, `smartSignInRedirect` is called, and if `authToken` is set, it transitions to `Auth.resolveRedirectRoute('signIn')`.

## Auth.SignOutController

Similar to SignInController, except redirects to `signOut` when `authToken` is reset (cleared).

## Auth.Module.RememberMe

Remember me functionality (only tested for Devise). Methods:

* init
* recall
* remember
* forget

When initialized, it will setup triggers, to call `remember` when the user signs in and to call `forget` when the user is signed out or has sign in error.

The remember functionality uses the jquery cookie library, also included with this gem.

The Remember me functionality can be cofigured via the `Auth.Config` object. The defaults are:

* rememberMe: false,
* rememberTokenKey: null,
* rememberPeriod: 14

You should set `rememberMe` to `true` and some `rememberTokenKey` string value to turn on RememberMe functionality.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
