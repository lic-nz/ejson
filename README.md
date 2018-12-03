[![Docker pulls](https://img.shields.io/docker/pulls/rubygem/ejson.svg)](https://hub.docker.com/r/locksmithdon/ejson/) [![Docker Build](https://img.shields.io/docker/automated/rubygem/ejson.svg)](https://hub.docker.com/r/locksmithdon/ejson/) [![Latest Tag](https://img.shields.io/github/tag/don-smith/ejson.svg)](https://hub.docker.com/r/locksmithdon/ejson/) [![Gem Downloads](https://img.shields.io/gem/dt/ejson.svg)](https://rubygems.org/gems/ejson/)

This is a Docker image that contains an installation of [`ejson`](https://github.com/Shopify/ejson) that lets you use this Ruby tool without the need to install Ruby or this gem on your machine.

This image is generated automatically by Docker Hub from the `master` branch of a GitHub repository. This ensures you know exactly what is in the image and what is not.

# Usage

**Note**: On Windows, replace `${PWD}` below with the full path to the present working directory. For example, instead of

```
docker run -v ${PWD}:/keydir -it --rm locksmithdon/ejson keygen -w
```

you would run

```
docker run -v C:/Users/me/project:/keydir -it --rm locksmithdon/ejson keygen -w
```

## Create a key pair

```
docker run -v ${PWD}:/keydir -it --rm locksmithdon/ejson keygen -w
```

**Important**: Map your present working directory to the container's `/keydir` folder so you have access to the generated key pair. This command will create a file with a long name. The file name is the value of the public key. The private key is the contents of the file.

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
docker run -v ${PWD}:/keydir -it --rm locksmithdon/ejson encrypt secrets.ejson
```

This will encrypt any plaintext fields and leave any encrypted fields unmodified. This command modifies the file in place.

## Decrypt an ejson file

```
docker run -v ${PWD}:/keydir -it --rm locksmithdon/ejson decrypt secrets.ejson
```

This will decrypt the file and print the contents to stdout. In order for this command to succeed, you must have the key pair (created with `keygen -w` above) in the present working directory. This command will _not_ modify the file in place.

# References

- [Ruby Gems](https://rubygems.org/gems/ejson/)
