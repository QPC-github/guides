---
layout: default
title: MFA requirement opt-in
url: /mfa-requirement-opt-in
previous: /using-mfa-in-command-line
next: /using-s3-source
---
<em class="t-gray">How to opt-in for MFA requirement.</em>

You can make your gems more secure by requiring that all privileged
operations by any of the owners require OTP.

## Opt-in to MFA requirement

You can opt-in a gem you are managing by releasing a version that has
`metadata.rubygems_mfa_required` set to `true`.

    % cat hola.gemspec
    Gem::Specification.new do |s|
    ...
    s.metadata       = { "rubygems_mfa_required" => "true" }
    ...
    end

The version being released with `rubygems_mfa_required` set and all the following version
will require that you provide an OTP for all privileged operations.
Once enabled, the gem page will show `REQUIRES MFA ACCOUNTS: Since <version>` in the sidebar:
    ![REQUIRES MFA ACCOUNTS](/images/mfa-required-since.png){:class="t-img t-img--small"}

You will see the following error message if you have not enabled MFA and you are trying to release
a new version for a gem that requires MFA:

    $ gem push hola-1.0.0.gem
    Pushing gem to https://rubygems.org...
    Rubygem requires owners to enable MFA. You must enable MFA before pushing new version.


## privileged operations

Following operations will require OTP verification if you have MFA requirement
set on the gem.

- `gem push`
- `gem yank`
- `gem owner --add/remove`
- **adding or removing owners using gem ownership page**

## Disabling MFA requirement

You can disable the MFA requirement by setting `rubygems_mfa_required` to `"false"` or any [`ActiveRecord::Type::Boolean::FALSE_VALUES`](https://api.rubyonrails.org/classes/ActiveModel/Type/Boolean.html).

**Note:** We will enforce the MFA requirement on the version being published. MFA requirement will be disabled after you have successfully
published a gem with rubygems_mfa_required set to false.
