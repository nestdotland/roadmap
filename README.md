# Roadmap

Roadmap for upcoming features.

## Repos

- nest.land: website front-end
- cdn: Gateway to arweave and cdn
- api: API
- proxy: Serverless function for "magic" link
- cli: Nest CLI (and webview GUI)
- eggs: package manager
- docs: Documentation
- dmca: DMCA requests

## Notes

#### Q/A

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

- `hatcher` and `yolk` should be merged into `eggs` and `cli` respectively, though still be usable as modules.
