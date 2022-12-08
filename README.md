# Multi-Instance Serverless Lambda Example

This example demonstrates how to deploy multiple instances (or copies) of the same lambda function by using the "--param" Serverless option to pass in an instance name. The main idea is to rename AWS resources and outputs which would otherwise clash using the instance name. Each lambda's configuration can be made instance-specific by including the instance name in environment variable or parameter store keys. The benefit of this approach is that only one lambda function needs to be defined in `serverless.ts` (or `serverless.yml`), but any number of deployments of it can be made.

## Setup

1. Install Node.js 18 (recommended: [Node Version Manager](https://github.com/nvm-sh/nvm#install--update-script)).

2. Install packages:

```bash
npm i
```

## Use case

- Any case where multiple lambda functions are needed and their implementation is identical, but you don't want to explicitly define each instance in the `serverless.yml` file.

## Usage

### Deployment

To deploy an instance "foo" of the lambda function `hello` to the default `dev` stage environment in the `ap-southeast-2` region , run:

```bash
npx sls deploy --region ap-southeast-2 --param="instance=foo"
```

Another instance, "bar", can then be deployed with:

```bash
npx sls deploy --region ap-southeast-2 --param="instance=bar"
```

Two separate instances of `hello` are now deployed on AWS:

<img src="./.images/aws-lambda-screenshot.png" alt="Both instances of `hello` are deployed." width="600"/>
<br>
<br>

The "--param" option is required to view information about a specific instance:

```bash
npx sls info --region ap-southeast-2 --param="instance=foo"
```

### Invocation

Invoke an instance of `hello` by passing the "--param" option:

```bash
npx sls invoke -f hello --region ap-southeast-2 --param="instance=foo"
```

Output:

```
{
    "statusCode": 200,
    "body": "{\n  \"message\": \"Function `aws-node-typescript-multi-instance-lambda-foo-dev-hello` executed successfully.\",\n  \"input\": {}\n}"
}
```

### Removal

The "--param" option is also required to tear down the stack associated with a `hello` instance.

```bash
npx sls remove --region ap-southeast-2 --param="instance=foo"
```

## Acknowledgements

Inspired by a [comment](https://github.com/serverless/serverless/issues/9361#issuecomment-884602588) by [**rdemorais**](https://github.com/rdemorais)'s on Serverless issue [Output collision when deploying a split service into the same bucket #9361](https://github.com/serverless/serverless/issues/9361).
