This is a Docker image that contains an installation of [`ejson`](https://github.com/Shopify/ejson) that lets you use this Ruby tool without the need to install Ruby or this gem on your machine.

This image is currently manually maintained by pushing to the `lic/ejson` repository in Amazon ECR.

# Safety

**DO NOT** place the file containing your private secret inside the working directory (git repository). They should be stored somewhere outside of a Git repository such that they cannot be mistakenly committed. An `~/.ejson/keys` directory is recommended (`~` being `C:/Users/yourusername` in Windows).
The example commands in this document make use of such a directory.

# Usage

**Note**

Throughout these instructions an image name `lic/ejson` is used to keep things short, but this will not work by default. You can either:

- Run `docker pull 459425154642.dkr.ecr.ap-southeast-2.amazonaws.com/lic/ejson` followed by `docker tag 459425154642.dkr.ecr.ap-southeast-2.amazonaws.com/lic/ejson lic/ejson` before continuing,

or

- Replace occurences of `lic/ejson` with the full repository URI `459425154642.dkr.ecr.ap-southeast-2.amazonaws.com/lic/ejson`.

**Note**: On Windows, replace `${PWD}` below with the full path to the present working directory. For example, instead of

```
docker run -v C:/Users/me/.ejson/keys:/keydir -v ${PWD}:/secretsdir -it --rm lic/ejson encrypt ../secretsdir/secrets.tfvars.ejson
```

you would run

```
docker run -v C:/Users/me/.ejson/keys:/keydir -v C:/my-project/infra/non-prod/dev/services/my-service:/secretsdir -it --rm lic/ejson encrypt ../secretsdir/secrets.tfvars.ejson
```

## Create a key pair

```
docker run -v C:/Users/me/.ejson/keys:/keydir -it --rm lic/ejson keygen -w
```

**Important**: Map your keys working directory to the container's `/keydir` folder so you have access to the generated key pair. This command will create a file with a long name. The file name is the value of the public key. The private key is the contents of the file.

## Create an ejson file

For example, consider the following file named `secrets.ejson`:

```
{
  "_public_key": "<key>",
  "password": "1234password"
}
```

Replace `<key>` with the value of your generated public key. For some important considerations, read more about the [format of the ejson file](https://github.com/Shopify/ejson#format). The public key must be in a field named `_public_key`.

## Encrypt an ejson file

```
docker run -v C:/Users/me/.ejson/keys:/keydir -v ${PWD}:/secretsdir -it --rm lic/ejson encrypt ../secretsdir/secrets.tfvars.ejson
```

This will encrypt any plaintext fields and leave any encrypted fields unmodified. This command modifies the file in place.

## Decrypt an ejson file

```
docker run -v C:/Users/me/.ejson/keys:/keydir -v ${PWD}:/secretsdir -it --rm lic/ejson decrypt ../secretsdir/secrets.tfvars.ejson
```

This will decrypt the file and print the contents to stdout. In order for this command to succeed, you must have the key pair (created with `keygen -w` above) in your `~/.ejson/keys` directory. This command will _not_ modify the file in place.

# References

- [Ruby Gems](https://rubygems.org/gems/ejson/)
