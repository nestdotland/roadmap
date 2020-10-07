# Nest Specs

**NOTE**: This spec is in no way official (but kinda is now?) and doesn't guarantee the presence or development of any mentioned features. This should be seen as a list of features I (@maximousblk) personally would like in Nest. I have not included many things here because either I don't know enough about them or I forgot.

## Website

Design: [figma.com/file/BfYs1TMPaEx8Mr9haEPE6H](https://figma.com/file/BfYs1TMPaEx8Mr9haEPE6H) (incomplete)

#### Stack

- [NextJS](https://nextjs.org)
- [Tailwind CSS](https://tailwindcss.com)
- [Chakra UI](https://chakra-ui.com)
- [NextAuth.js](https://next-auth.js.org)
- [Docusaurus 2](http://v2.docusaurus.io)

#### UI

- Minimal
- Dark Theme
- Use MDX where possible

#### Auth

- Username/Password (primary)
- GitHub
- GitLab
- Email

The email will be necessary for 2FA and account recovery

#### Pages

###### Home

- Hero
- Description
- Features
- Get started
- Standard modules

MDX

###### User

- Avatar
- Username
- Name
- Bio
- Modules

TSX

###### X

- Search
- Featured modules
- All modules

TSX

###### Module

- Name
- File Explorer
- Readme

TSX

###### File

- Nest URL breadcrumbs
- Version selector
- Link for the latest version
- Link to raw (arweave) version
- Link to type docs
- syntax highlighted raw code

TSX

###### Blog

- Minimal MDX blog
- For detailed release notes and announcements

###### Post

- Title
- Date
- Reading time
- Content
- Edit link

MDX

###### Orgs

- Hero with logo, name and description
- Link to their homepage
- Link to their GitHub/GitLab
- List of all modules

TSX

###### Error

- Dino runner game

TSX

#### Routes

```yml
nest.land # home page
nest.land/u # 404
nest.land/u/:user # user profile & list of modules
nest.land/u/:user/settings # user settings

nest.land/x # user modules
nest.land/x/:user/:module # redirect to latest version
nest.land/x/:user/:module/:filepath # redirect to latest version
nest.land/x/:user/:module@:version # details for specific version
nest.land/x/:user/:module@:version/:filepath # filepath for specific version

nest.land/- # redirect to /x
nest.land/-/:module # vanity url for details for latest version (redirect)
nest.land/-/:module/:filepath # vanity url for filepath for latest version (redirect)
nest.land/-/:module@:version # vanity url for details for specific version
nest.land/-/:module@:version/:filepath # vanity url for filepath for specific version

nest.land/docs # homepage for docs
nest.land/docs/nest # website docs
nest.land/docs/eggs # eggs docs
nest.land/docs/cli # nest cli docs
nest.land/docs/api # api docs

nest.land/blog # latest posts and search
nest.land/blog/:slug # Blog post

orgs.land # info about the service
<org>.orgs.land # organisation profile and modules list
<org>.orgs.land/:module # details for latest version
<org>.orgs.land/:module/:filepath # filepath for latest version
<org>.orgs.land/:module@:version # details for specific version
<org>.orgs.land/:module@:version/:filepath # filepath for specific version
```

## Nest CLI

- Use the latest standard modules as much as possible
- Use ASCII for stdout as much as possible
- Store all config in `.nest` directory
- Exclude all dotfiles by default while publishing
- Enforce semver

#### Commands

```sh
nest register # opens the register page in browser

nest login # prompt for login info
nest login --token <token> # add access to an account

nest # asks to initialize a new module or link an existing one
     # and if a module is initialized and linked, just sync the metadata

  # Nest CLI 1.0.0
  # ? setup "~/mod"? [Y/n]:
  # ? link to an existing module? [Y/n]:
  # ? what's the name of your existing module? (mod):
  # i linked to "user/mod" (created .nest and added it to .gitignore)

  # Nest CLI 1.0.0
  # ? Setup "~/mod"? [Y/n]:
  # ? Link to an existing module? [Y/n]: n
  # i Setting up a new project
  # ? Module name (mod):
  # ? Version (0.1.0):
  # ? Description:
  # ? Homepage:
  # i Linked to "user/mod" (created .nest and added it to .gitignore)

nest settings # opens user settings in editor

nest config # opens module config in editor

nest publish [ <version> || pre <id | alpha> || patch || minor || major ] # publishes the specified version

nest audit # returns an audit report

# to do any task in the gui, add a --gui or -G flag
```

#### Configuration

```jsonc
// .nest/egg.json

{
  "name": "mod",
  "full_name": "My Awesome Module",
  "description": "I swear my module is the best out there",
  "version": "x.y.z-meta",
  "homepage": "https://my.awesome.module",
  "license": "LICENSE",
  "entry": "mod.ts",
  "unlisted": false
  // ...
}
```

```gitignore
# .nest/nestignore

@extends gitignore

docs/
tests/
CHANGELOG
CODE_OF_CONDUCT.md
CONTRIBUTING.md
funding.yml
!.shared_dot_file
!.shared_dot_directory/
```

```markdown
<!-- .nest/README.md -->

> Why do I have a directory named ".nest" in my project?

The ".nest" directory is created when you link a directory to a Nest module.

> What does this directory contain?

The "egg.json" file contains metadata about the module

The "nestignore" file contains a list of files that are ignored or included

> Should I commit the ".nest" directory?

No, you do not need to commit the ".nest" directory to your repository.
Upon creation, it will be automatically added to your ".gitignore" file.
```

#### GUI

- Use webview_deno (or open the default browser) on port 3995 (leet speak for eggs)
- Able to do everything CLI and website combined

## Eggs

- Module manager only
- Support semver constraints (similar to [udd](https://github.com/hayd/deno-udd))

#### Commands

```sh
eggs --nest-token <token> # adds nest token to use while caching private modules

eggs <target_file> [options] # caches all (public/private) the dependencies
    Options:
        -m, --map              use an importmap instead
        -l, --lockfile         consider lockfile

eggs cache <target_file> [options] # caches all (public/private) the dependencies
    Options:
        -m, --map              use an importmap instead
        -l, --lockfile         consider lockfile

eggs add [options] [ <target_file> or deps.ts ] [modules] # add export to the target file
    Options:
        -r, --registry         name of the registry to use (default: nest)
        -m, --map              use an import map instead

eggs update [options] [ <target_file> or deps.ts ] # bump all the modules to the specified limit
    Options:
        -r, --registry         name of the registry to lookup
        -m, --map              use an import map instead

eggs uninstall [options] [ <target_file> or deps.ts ] [modules] # removes export from the target file
    Options:
        -m, --map              use an import map instead

eggs purge [options] [ <target_file> or deps.ts ] # analyse and remove unused deps
    Options:
        -m, --map              use an import map instead

eggs upgrade # update eggs to the latest version
```

## API

- GraphQL API
- REST API

```yml
gql.nest.land # graphql api
api.nest.land # rest api
```

## Functions

- Use [Next.js API](https://nextjs.org/docs/api-routes/introduction)
- Functions:
  - Magic URL
  - GraphQL wrapper
  - Webhooks

#### Magic URL

- Same URL for web and import
- Handle `X-Deno-*` headers
- Proxy arweave and cache data on Vercel

#### Rest API

- GraphQL Wrapper
- Read-only API

###### Endpoints

```yml
api.nest.land/u # list of all users
api.nest.land/u/:user # user profile
api.nest.land/u/:user/modules # list of modules by :user
api.nest.land/u/:user/settings # user settings

api.nest.land/x # list of all modules
api.nest.land/x/:user/:module # details for latest version
api.nest.land/x/:user/:module/:filepath # details of the filepath for latest version
api.nest.land/x/:user/:module@:version # details for specific version
api.nest.land/x/:user/:module@:version/:filepath # details of the filepath for specific version
```

#### Webhooks

- Support GitHub and GitHub

###### Endpoints

```yml
webhook.nest.land/github/:user/:module # publish from github
webhook.nest.land/gitlab/:user/:module # publish from gitlab
```

## Branding

- Name: Nest Land
- website: nest.land
- GitHub org name: nestland
- logo: _ref. design_

## Repos

- nest.land: website front-end
- api: API
- cli: Nest CLI (and webview GUI)
- eggs: package manager
- docs: Documentation
- dmca: DMCA requests

## Notes

#### Q/A

> Why Next.js and not vue/nuxt or svelte/sapper?

1. Incremental Static Regeneration (IRS)
2. We use Vercel for most of our user-facing infrastructure and Vercel has native support for Next.js
3. We also depend on Vercel's sponsorship and as per the sponsorship conditions, we cannot use SSR and using client-side rendering greatly impacts performance (and obviously SEO).
4. Generating static pages for all modules is impractical and client-side rendering is heavy irrespective of the framework.

> Why Tailwind and not vanilla CSS?

Because CSS frameworks handle many edge cases and multiple browser specs on their own. [MDN browser compatibility report](https://mdn-web-dna.s3-us-west-2.amazonaws.com/MDN-Browser-Compatibility-Report-2020.pdf)

> Why Chakra UI?

Because it's easier to use ready-made components as they too (similar to CSS frameworks) handle edge cases and different browser specs.

> Why NextAuth.js?

Native support for Next.js

> Why Docusaurus v2?

It looks and feels the best out of the box. choosing v2 because v1 looks super old.

> Why MDX and not JSX/TSX?

Just because JSX/TSX is super ugly to work with and you can use JSX inside MDX.

> Why `/x/:user/:module` and not `/x/:module`?

Because - again, personal opinion - I want to be able to publish a varients of an already existing module with the same name but under my account. Similar to how GitHub does it. you can have forks of an already existing but deprecated modules.

Also there are only so many sensibly short combinations of alpha numeric characters that make sense as a name. And there is no way to publish scoped modules without a slash in the name and `@` is already used for version.

This also helps with "name sqatting" and "typos quatting" which have been an issue with npm in the past. [Recent incident of typo squatting on npm](https://zdnet.com/article/four-npm-packages-found-uploading-user-details-on-a-github-page)

> Why is `/-/:module` a vanity route?

It is to make sure that these modules are of good quality and trust. So whenever you see a module with that route, you know it's good. To get a vanity URL, a module will have to meet certain criteria that ensures code quality and authenticity. A vanity route will be granted after a manual review.

Criterion:

- Properly formated
- properly linted
- Properly documented
- Includes an explicit license
- 90%+ test coverage

Any module that starts disobeying this criterion will be prohibited from publishing to the vanity route. they can still publish to their own username and old versions on the vanity route will be available to use for any project that uses it.

> Why are Nest CLI and eggs separate?

Nest CLI is the main utility you would use to create and publish modules. eggs on the other hand, is just a reference module manager that has first-class support for Nest including private modules. Other module managers can use this reference to integrate Nest into their software.

> Why Next.js API routes and not Vercel Functions

Because Vercel Functions are proprietary and in case we lose the sponsorship or need to migrate to some other platform, we would have to rebuild the functions. Using Next.js would mean we can run those functions on any server.

> What is that `.nest` directory? what happened to the whole "no package.json" thing.

Yes, the `.nest` contains the configurations specific to Nest. But the point is that always having to plug all the config options through flags is not really that great user experience. You don't have to and it is not recommended to push this directory to git hosting services. It is not a part of your code. It will automatically be downloaded to your project directory when setting up and later you can remove it. What this does is, makes it easy to configure your module through a dedicated config file and while being able to keep it away from the project source code.

> Why would you need a REST API when you have a GraphQL API?

Simply put, REST is more accessible to beginners that GraphQL. Having more options is not a bad thing to have. Also the REST API would simply be a read-only wrapper function for GraphQL API. No need for a full API server.

> What is this "Magic URL" you mention?

I don't know what to call it. It is just a function that checks if the client is a browser and sends raw data or webpage accordingly. similar to what `deno.land/x` does.

> why "Nest Land" as the name and not "nestdotland"?

I don't think punctuation should be part of the name. And the name "Nest - Land" is simpler to use than "Nest - Dot - Land".

#### Authentication

- Primary way to login would be using a username and password paired with [HCaptcha](https://hcaptcha.com) for human verification.
- Secondary options would be to use GitHub or GitLab to login. This would restrict you to only using the auth token when logging into the Nest CLI. Though you can add a password or connect your GitHub/GitLab account anytime.
- Email can be added and used to login. A link will be sent to this email for you to login. Emails can also be used for 2FA and Account recovery, in case you forgot your username/password or your GitHub/GitLab account gets some issue.

> But what about GDPR?

GDPR doesn't restrict the collection of user info. The only thing restricted is the sharing of that info with third parties without the user's explicit consent. So all we need to do is make the database secure and not sell user data.

#### Analytics

For analytics, use [Minimal Analytics](https://minimalanalytics.com/) instead. It is Google analytics just way less "creepy" and super light weight (GA: 73KB, mGA: 1.5kb).

Module import analytics may be the hardest part to implement correctly. We absolutely don't want someone to just do `watch curl <Module URL>` and get paid for that.

Checking user agent for Deno won't work cuz you can do `watch deno cache -r <Module URL>`

So we need the analytics to be bound to the IP address of the user and have a cooldown before that IP can be counted as a real user. I'd suggest one or two days cuz you don't need to refresh your cache that often.

But with that approach, there is a problem of people abusing the CI/CD services as every instance has a separate IP. You could write a cron job to deno cache the URL every minute.

An obvious question arises here:

> How does NPM does it? they got legit download metrics, right?

nope. They themselves claim that [the numbers are not accurate](https://blog.npmjs.org/post/92574016600/numeric-precision-matters-how-npm-download-counts) and having accurate analytics is hard.

tl;dr: they count every `HTTP 200` request as a download.

Same for GitHub, PyPi and almost all kinds of registries. This is because they didn't need it to be accurate.

Now someone would ask,

> how does YouTube count views? I tried some bots, they filter it out just fine.

agree. So does Google analytics or any analytics service for that matter. But the two products and distribution methods are fundamentally different.
They have the advantage of being on a website that can run javascript code on the browser to figure out if the client is a bot. an HTTP server has no such feature.

This is the whole request that is sent to the URL from Deno when caching or running a remote file:

```
> GET /eggs@0.2.3/mod.ts HTTP/1.1
> Accept: */*
> User-Agent: curl/7.55.1
> Accept-Encoding: gzip, br
> Host: x.nest.land
```

There can be no captcha, no cookie, no tracking, nothing to differentiate a bot from a legit user. And when there is something you cannot define but you still want to identify, you turn to AI-assisted solutions, which we would need to create then train, which we don't have training data or expertise for.

At this point I think we should look for another metric for deciding the payout.

#### Private modules

Because of the nature of blockchain, we cannot keep the data "hidden". The option left is encrypting the data. furthermore we can push the data to arweave in multiple pieces making it harder to put together the encrypted data.

Publishing

- Ensure the connection is HTTPS
- upload tarball to Nest
- encrypt tarball
- upload to arweave

Using

- Run `eggs cache` (or use any module manager that supports private Nest modules)
- Ensure the connection is HTTPS
- Send a GET request to Nest. The request should contain an array of access tokens currently linked through the cli as a query or `X-Nest-Tokens` header
- Nest will fetch the requested file from arweave and decrypt it
- Nest will return the unencrypted file to the module manager
- The module manager will then put the fetched file in the deno cache directory.

#### Copyright

All public modules published on Nest assume you want people to be able to use them freely (freedom 0 only). Further license info **MUST** be included in the readme and the module should be published with a `LICENSE` file.

The license info will be available through the API. For that the `LICENSE` file should be in the root directory of the module. Then the license will be analyzed at publish time. If it is an unknown license, the API will return `UNKNOWN` for the license name.

If a module is reported for copyright infringement through a legit DMCA (or equivalent) request, the module should be immediately unlisted and blocked from being accessed or updated. Then the module should be made inaccessible through nest (still accessible through arweave and listed in the API) and a clear notice should be displayed on the module's page and the cli using the `X-Deno-Error` header. Then if the user wants to dispute the claim, they can request a manual review from the Nest team (not sure what happens here). And if someone wants to access the module through Nest for a legit reason, they will be granted a limited access key on request, which can be used the same way as private repositories do. After the request has been accepted and enacted, the request documents should be pushed to a public `dmca` repository where people can refer to. This is to ensure that people don't blame us for removing content without reason.

Now the obvious question arises:

> You promised to be immutable!?

Yes, by definition all the modules are still immutable as they cannot be edited (mutated) but in extreme cases we need to take action against bad actors to prevent lawsuits against us. And as we cannot remove it from the blockchain, all we can do is block access through our gateway to the artifact on the blockchain.

#### misc

- Blog will be used to post detailed release notes and obviously for announcements.

- Anything that could lead to unwanted consequences, should be handled by the Nest GUI or the website. These features should never be able to be automated. These should require human interaction.

- Module authors can apply to manually verify a module. Only verified modules will be featured at the top of the "modules" page. "Verification" here means that we check if the module isn't malicious or a "not so useful" module.

- `hatcher` and `yolk` should be merged into eggs though still be usable as modules.

- Lazy load the lists on requests i.e. when the client presses "Load more" (ref. design).
