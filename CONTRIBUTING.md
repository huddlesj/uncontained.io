# Contributing to Uncontained.io

Thanks for checking out the uncontained.io guides! We're excited to hear and learn from you. Your experiences will benefit others who read and use these guides.

We've put together the following guidelines to help you figure out where you can best be helpful.

## Table of Contents

0. [Types of contributions we're looking for](#types-of-contributions-were-looking-for)
0. [Ground rules & expectations](#ground-rules--expectations)
0. [How to contribute](#how-to-contribute)
0. [Style guide](#style-guide)
0. [Setting up your environment](#setting-up-your-environment)
0. [Contribution review process](#contribution-review-process)
0. [Community](#community)

## Types of contributions we're looking for
This project is an attempt at establish and document some guiding principles and, dare we say it, best practices in the open source emerging technology space. Currently that space, as we see it, is predominantly made up of technologies that enable DevOps practices, such as "Cloud" (API-automated Infrastructure), CI/CD tooling, container technologies, microservices frameworks, and probably more. This project was started by a community of thought leaders from Red Hat so if it initially appears as if this site is only about Red Hat technologies, it's not intended to be, its just impassioned people writing about what they know.

There are also many ways you can directly contribute to the guides (in descending order of need):

* Fix editorial inconsistencies, inaccuracies, or stale content
* Add tutorials, examples, or anecdotes that help illustrate a point
* Revise language to be more approachable and friendly
* Propose a new guide ([here's how](./docs/new_guides.md))

Interested in making a contribution? Read on!

## Ground rules & expectations

Before we get started, here are a few things we expect from you (and that you should expect from others):

* Be kind and thoughtful in your conversations around this project. We all come from different backgrounds and projects, which means we likely have different perspectives on "how open source is done." Try to listen to others rather than convince them that your way is correct.
* Uncontained.io guides are released with a [Contributor Code of Conduct](./CODE_OF_CONDUCT.md). By participating in this project, you agree to abide by its terms.
* If you open a pull request, please ensure that your contribution passes all tests. If there are test failures, you will need to address them before we can merge your contribution.
* When adding content, please consider if it is widely valuable. Please don't add references or links to things you or your employer have created as others will do so if they appreciate it.

## How to contribute

If you'd like to contribute, start by searching through the GitHub [issues](https://github.com/redhat-cop/uncontained.io/issues) and [pull requests](https://github.com/redhat-cop/uncontained.io/pulls) to see whether someone else has raised a similar idea or question.

If you don't see your idea listed, and you think it fits into the goals of this guide, do one of the following:
* **If your contribution is minor,** such as a typo fix, open a pull request.
* **If your contribution is major,** such as a new article or category, start by opening an issue first. That way, other people can weigh in on the discussion before you do any work.

### Contributing a new article

See our guide on [contributing new content](./docs/new_articles.md).

### Migrating Content from OpenShift-Playbooks

This site is the evolution of the [Openshift Playbooks](https://github.com/redhat-cop/openshift-playbooks) site.

Migrating content from there to here is fairly straightforward, but involves some work.

1. A migration script can be used to translate documents from the [openshift-playbooks](https://github.com/redhat-cop/openshift-playbooks) repo.

    Usage:
    ```
    ./migrate-doc.sh [path/playbook-doc] [path/uncontained-doc]
    ```

    Example:
    ```
    ./migrate-doc.sh continuous_delivery/external-jenkins-integration.adoc articles/external-jenkins-integration.adoc
    ```
2. Next, start the test site using the [container image](#containerized-hugo-environment-quickstart-guide) or [native hugo](#native-hugo-environment-quickstart-guide).
3. Navigate to the content you migrated and ensure that:
  * all content renders correctly
  * there are no broken links (we recommend using a link checker like [this one for Firefox](https://addons.mozilla.org/en-US/firefox/addon/linkchecker/) or [this one for Chrome](https://chrome.google.com/webstore/detail/broken-link-checker/nibppfobembgfmejpjaaeocbogeonhch?hl=en))
  * if the guide or article links to other guides, or articles, we suggest you migrate those as well.
4. Examine front matter. Ensure `title` is reader friendly, and that the `date` represents the date written, not the date migrated.

### Helping us with Site Styling, User Experience, etc.

The styling for the site is contained within a [Hugo Theme](https://gohugo.io/themes/). The source code for the theme is located in this repo at [./site/themes/uncontained.io](./site/themes/uncontained.io).

Specifically, the code for our HTML Layouts can be found in [./site/themes/uncontained.io/layouts/](./site/themes/uncontained.io/layouts/). The CSS code can be found in [./site/themes/uncontained.io/src/scss](./site/themes/uncontained.io/src/scss/).

See [Building and Developing the site](#building-and-developing-the-site) for instructions on how to perform local testing of changes.

## Style guide

This site is written in asciidoc format and is built using [Asciidoctor](https://asciidoctor.org/). If you're writing content, see the [style guide](./docs/style_guide.adoc) to help your prose match the rest of the Guides.

## Setting up your environment

This site is powered by [Hugo](https://gohugo.io/). Running it on your local machine only requires a working [Hugo](https://gohugo.io/getting-started/installing) installation, either natively or a docker container. We recommend using the docker container method (for obvious reasons) instead of your native operating system, but you may choose either environment setup.

### Building and Developing the Site

Uncontained.io uses the [Hugo static site generator](https://gohugo.io/) to translate Asciidoc into an HTML site. We wrap this functionality in [Gulp](https://gulpjs.com/) to streamline the build and test process. Running the site requires [Node.js](https://nodejs.org/). Once installed, you should have all you need to build, test and develop. To get started, set up your local environment.

```
git clone https://github.com/redhat-cop/uncontained.io.git
cd uncontained.io/
npm install
```

Our Hugo + Gulp setup is based on the [victor-hugo](https://github.com/netlify/victor-hugo) boilerplate.

#### Run the Live Preview

The live preview provides an embedded server in which you can test the site locally, and watch changes being made as you develop.

```
npm start
```

or, if you'd like to also include _draft_ content like `hugo server --buildDrafts --buildFuture`:

```
npm run start-preview
```

You should be able to view the site by browsing to http://localhost:3000/.

#### Build the Site

The site build will simple generate all of the site html, css, javascript, etc. This is the process we use to build and publish the site.

```
npm run build
```

#### Deploying to OpenShift

Uncontained.io is built and hosted on OpenShift, and deployed using [OpenShift Applier](https://github.com/redhat-cop/openshift-applier)

Run the following to pull in applier:

```
ansible-galaxy install -r requirements.yml -p galaxy
```

Log into your _Prod_ OpenShift cluster, and run the following command.
    ```
    $ oc login <prod cluster>
    ...
    $ ansible-playbook -i .applier-prod/ galaxy/openshift-applier/playbooks/openshift-cluster-seed.yml
    ```
One of the things that was created by ansible is a `ServiceAccount` that will be used for promoting your app from _Dev_ to _Prod_. We'll need to extract its credentials so that our pipeline can use that account.
    ```
    $ TOKEN=$(oc serviceaccounts get-token promoter -n field-guides-prod)
    ```
We need to create the the *prod-api-credentials* param file so our pipeline will be able to verify a successful deployment to production.
    ```
    $ echo "TOKEN=${TOKEN}
    API_URL=<API_URL>
    REGISTRY_URL=<REGISTRY URL>
    SECRET_NAME=other-cluster-credentials" > .openshift/params/prod-cluster-credentials
    ```

Now, deploy your pipeline and dev environment to your _development_ cluster:

```
oc login <dev cluster>
ansible-playbook -i .applier/ galaxy/openshift-applier/playbooks/openshift-cluster-seed.yml
```

## Contribution review process

This repo is currently maintained by @jaredburck, @etsauer, and @sabre1041, who have commit access. They will likely review your contribution. If you haven't heard from anyone in 10 days, feel free to bump the thread or @-mention a maintainer or `@redhat-cop/cant-contain-this` to review your contribution.

## Community

Discussions about uncontained.io takes place within this repository's [Issues](https://github.com/redhat-cop/uncontained.io/issues) and [Pull Requests](https://github.com/redhat-cop/uncontained.io/pulls) sections and Trello's [Cant-Contain-This](https://trello.com/b/JMaxIjCy/cant-contain-this) board. Red Hatter's can also find us on RocketChat's #cant-contain-this channel. Anybody is welcome to join these conversations. There is also a [mailing list](http://uncontained.io/) for regular updates.

Wherever possible, do not take these conversations to private channels, including contacting the maintainers directly. Keeping communication public means everybody can benefit and learn from the conversation.
