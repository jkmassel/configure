# Configure

A tool for storing encrypted secrets in your repository, and decrypting them in CI. It allows you to store your configuration files in your source repository, even if that repository is public.

## How it works
This tool assumes you want to copy your configuration file (such as API keys for third-party services) into your project in an encrypted form. When running on developer machines or CI, you'll then be able to decrypt the files when building.

The configuration is specified in a `.configure` file at the root of your project. It defines the following fields:

**Project Name**
The `project_name` field matches a key in a `keys.json` file defined in the root of your secrets repository. That file contains the encryption/decryption key for the project.

**Branch**
The default branch to pull new secrets from when running `configure update`. 

**Pinned Hash**
The `pinned_hash` refers to the commit hash associated with the version of the secrets current in use for this project.

**Files to Copy**
The `files_to_copy` is a list of file hashes, each containing a `file` and `destination` key. The `file` key is the path to the file relative to the secrets repo root. The `destination` key is the path to where the file should be placed relative to the project root.

A sample `.configure` file looks like:

```json
{
  "project_name": "my-sample-project",
  "branch": "main",
  "pinned_hash": "f6b64d80449499a0e45ba5ef2cb53b0179a9b8b8",
  "files_to_copy": [
    {
      "file": "gradle.properties",
      "destination": "my-project/gradle.properties"
    }
  ]
}
```

## How to use it

The configure tool has two main jobs: copy plain-text secrets files from your secrets repository into the project as encrypted blobs, and decrypting those blogs back into the plain-text files on developer and build machines.

### Setup

`configure init` will walk you through the process of setting up your project. It will:

- Create the `.configure` file for you
- Create an entry in your `keys.json` file for the project (if one doesn't already exist)
- Prompt you to add files from the secrets repo to the the `.configure` file
- Run the initial encryption process on the files provided, storing them in the repository
- Run the initial decryption process on the files proided, making the project ready for development

### Update

`configure update` is used to update the encrypted secrets in the project to the latest version in the secrets repo.


### Apply
`configure apply` is used to decrypt the secrets in the project and apply the decrypted secrets to their destination.