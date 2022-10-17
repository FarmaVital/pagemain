<div align="center">
  <h1>Nhost Netlify Starter Template</h1>
</div>

This is an example frontend for the multi-conference example app created with Nhost. It uses Next.js, React Query, Tailwind, Nhost as the backend (authentication, Postgres Database, GraphQL API) and Netlify to host the frontend.

You can check out the video walkthrough of the project [here](https://user-images.githubusercontent.com/310881/193014496-34859a88-19b4-4acd-819d-17a5745e0aec.mp4).

## Table of Contents

- [Features](#features)
- [Previews](#previews)
- [Project Structure Walkthrough](#project-structure-walkthrough)
- [Getting Started](#getting-started)
  - [Setting up local frontend](#setting-up-local-frontend)
  - [Setting up local backend with the Nhost CLI](#setting-up-local-backend-with-the-nhost-cli)
- [GraphQL API Schema and Example Queries](#graphql-api-schema-and-example-queries)
- [Deploy to Nhost](#deploy-to-nhost)
- [Deploy to Netlify](#deploy-to-netlify)


### Features

- [Next.js](https://github.com/vercel/next.js/)
- [Tailwind v3](https://tailwindcss.com/) for styling.
- [React Query v4](https://github.com/TanStack/query).
- [TypeScript](https://typescriptlang.org) static type-safety.
- [ESLint](https://eslint.org) linting & [Prettier](https://prettier.io) code formatting.
- [GraphQL](https://graphql.org/) and [GraphQL Code Generator](https://www.graphql-code-generator.com/).

### Previews

- Backend: https://rhhvmcjwixtxiumktygi.nhost.run/console
- Front-end: https://fanciful-concha-7412d8.netlify.app/

### Project Structure Walkthrough

Inside this project, you are going to see both the specification for the backend (Nhost) and the frontend (Next.js):

```
/
├── nhost/
├── .nhost/
├── src/
│   ├── components/
│   │   ├── common/
│   |   ├── conferences/
│   |   ├── speakers/
│   |   └── talks/
│   ├── graphql/
│   │   └── *.gql
│   ├── pages/
│   |   └── *.tsx
│   ├── types/
│   │   └── *.ts
│   └── utils/
│       ├── __generated__/
│       │   └── graphql.ts (contains auto-generated GraphQL hooks)
│       └── *.ts
└── package.json
```

- `nhost` is the main specification of your backend: tables, permissions & roles.
- `.nhost` is a folder created by the CLI that contains the state of your backend.
- `src` is the main folder for your frontend.
- `src/components` contains all the components used in the application.
- `src/graphql` contains all the hooks for GraphQL queries and mutations used in the application.
- `src/pages` contains all the pages used in the application.
- `src/types` contains all the types used in the application.
- `src/utils` contains all the utility functions used in the application.

### Getting Started

<details><summary><h4>Setting up local frontend</h4></summary>

1. Clone the repository:

```sh
git clone https://github.com/nhost/nhost-netlify-starter-nextjs-reactquery.git
```

2. Install the dependencies:

```sh
yarn install
```

3. Start the Next.js application:

```sh
yarn dev
```

</details>

<details><summary><h4>Setting up local backend with the Nhost CLI</h4></summary>

When you start developing your front-end you will see that there's data already preloaded. This is coming from an environment Nhost has prepared to run the `conference` project. In order to make changes to the backend (tables, columns, permissions, etc...) you need to start a local Nhost environment yourself.

1. You must install [Docker](https://docs.docker.com/get-docker/) before you can use the Nhost CLI. Make sure to have Docker running before proceeding.

2. Install the [Nhost CLI](https://docs.nhost.io/platform/overview/get-started-with-nhost-cli):

```sh
sudo curl -L https://raw.githubusercontent.com/nhost/cli/main/get.sh | bash
```

3. Start the Nhost project:

```sh
nhost up
```

> The CLI uses seed data (`nhost/seed`) to populate the database with a user and a conference. Learn more in the [Nhost CLI documentation](https://docs.nhost.io/platform/database#seed-data).

4. Create a `.env.local` file in the root with the following content:

```sh
NEXT_PUBLIC_NHOST_SUBDOMAIN=localhost:1337
```

5. Start (or restart) the Next.js application:

```sh
yarn dev
```

You'll see that the data is now coming from your local Nhost environment. Add conferences either through the [Hasura Console](http://localhost:9695) or through the [conference management page on the front-end](http://localhost:3000/conferences) by [signing in](http://localhost:3000/sign-in).

You can use the following credentials to manage conferences:

```sh
email: manager@conferenceplatform.com
password: Manager1234!
```

</details>

<details><summary><h3>GraphQL API Schema and Example Queries</h3></summary>

```graphql
query Conferences {
  conferences(order_by: { name: asc }) {
    id
    name
    slug
    location
    featured
    start_date
    end_date
    talks(order_by: { start_date: asc }) {
      id
      name
      start_date
      end_date
    }
    speakers(order_by: { name: asc }) {
      id
      avatar_url
    }
  }
}
```

```graphql
query Speakers {
  speakers {
    id
    name
    bio
    social
    job_description
    avatar_url
  }
}
```

```graphql
query Talks {
  talks {
    id
    name
    start_date
    end_date
    speaker {
      name
    }
  }
}
```

Queries and mutations defined in the `src/graphql` folder can be used in the front-end by generating the GraphQL hooks with the following command:

```sh
yarn codegen
```

This will generate the hooks in `src/utils/__generated__/graphql.ts`.

</details>

### Deploy to Nhost

<img align="left" width="35%" src="https://user-images.githubusercontent.com/20285232/181691897-1269d9d3-94fb-4958-ac27-83a70ab00309.png" >

[Nhost](https://nhost.io/) is an open source Firebase alternative with GraphQL. More importantly, by creating a project with Nhost you automatically get the following:

```
- Database: PostgreSQL.
- Instant GraphQL API: Hasura.
- Authentication: Hasura Auth.
- Storage: Hasura Storage.
- Serverless Functions: AWS Lambdas.
```

<details><summary><h5>Steps for creating an Nhost project</h5></summary>

Log in to your [Nhost Dashboard](https://app.nhost.io) and click the **Create Your First Project** button.

<p align="center" width="100%">
    <img width="55%"" src="https://docs.nhost.io/assets/images/nhost-dashboard-8e4da43291a39f8f9b127c470d75c079.png"> 
</p>

Next, give your new Nhost app a name, select a geographic region for your Nhost services and click Create App.

<p align="center" width="100%">
    <img width="55%" src="https://docs.nhost.io/assets/images/new-nhost-project-fc7763b2a8df9513ead5280e305bd554.png"> 
</p>

After a few seconds, you should get a PostgreSQL database, a GraphQL API with Hasura, file storage, and authentication set up.

**(Optional):** Any user you add to the project through the "Users" section of the Nhost Dashboard can access the manager dashboard of the `conference` project. You'll need to verify the email address of the user before they can sign in.

Learn more about email verification in the [Nhost documentation](https://docs.nhost.io/platform/authentication/sign-in-with-email-and-password#verified-emails).

</details>

<details><summary><h5>Connecting this repository to Nhost</h5></summary>

Nhost supports a Git workflow which means that you can safely work locally with the CLI and when you are ~~confident~~ with your changes, you can push to your repository and your project will be automatically deployed (any following updates you push to your code will also be automatically be deployed.) To allow this, you need to connect this repository to your Nhost projects through the Nhost Dashboard:

1. Fork/clone this repository to your GitHub account.

2. Provide the required permissions to select this repository for the official Nhost GitHub application.

3. Find your repository on the Nhost Dashboard & connect it:

<p align="center" width="100%">
    <img width="55%" src="https://user-images.githubusercontent.com/20285232/181070306-851187ca-6595-4cdc-b458-b62b479479db.png"> 
</p>

4. Once connected, the project will automatically deploy.

<p align="center" width="100%" height="100%">
    <img width="55%" src="https://user-images.githubusercontent.com/20285232/181070624-f12571a4-6b77-4a2f-acab-9e156306b392.png"> 
</p>

5. Add changes to your project. Any changes you push to your repository will also be automatically be deployed (you can see your deployments on the "Deployments" section of the console).

<p align="center" width="100%">
    <img width="55%" src="https://user-images.githubusercontent.com/20285232/181070633-c3c67e94-981c-4574-954b-c643448f387e.png"> 
</p>

And now once you're ready to deploy your frontend make sure to go back to the app overview and grab your Nhost Subdomain and Nhost Region:

<p align="center" width="100%">
<img width="33%" src="https://user-images.githubusercontent.com/20285232/181790261-065d7e61-6986-4acc-94d7-5a7f828da76d.png"> 
</p>

</details>
                                                                                                                                 
## Deploy to Netlify

Want to deploy this frontend immediately? Feel free to click on our Deploy to Netlify button to get you setup!

[![Deploy to Netlify Button](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/nhost/nhost-netlify-starter-nextjs-reactquery)

Clicking this button will start the setup for a new project and deployment

### Deploying through the command line

Clone this repo with the `git clone` command. Then install the [Netlify CLI](https://docs.netlify.com/cli/get-started/) tool and run `netlify init`.

```sh
git clone https://github.com/nhost/nhost-netlify-starter-nextjs-reactquery.git

npm install netlify-cli -g # to install the Netlify CLI tool globally

netlify init # initialize a new Netlify project & deploy
```

Once your Netlify site is deployed, add the environment variables from your Nhost project into the Netlify environment variables. You can find this from within the UI:
Site Settings > Build & deploy > Environment > Environment Variables

```
NEXT_PUBLIC_NHOST_SUBDOMAIN=YOUR_NHOST_APP_SUBDOMAIN
NEXT_PUBLIC_NHOST_REGION=YOUR_NHOST_APP_REGION
```
