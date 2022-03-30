# S3 Tarball Cloud Native Buildpack

This is a [Cloud Native Buildpack](https://buildpacks.io/docs/concepts/components/buildpack/)
that can download tarballs from private [Amazon S3](http://aws.amazon.com/s3/)
buckets. It gives you a way of adding private files outside the main git repository such
as certificates, and more complex attributes that won't fit on environment variables to
the container at build time without making it publicly accessible.

## How it works

This buildpack aims to allow you to write files in build time in any path within the root application
directory (`/app` or `/workspace` which is an alias). Therefore, with a tar archive you can achieve that
by defining the paths desired that will be expanded when downloaded. A second optional benefit is compression
when your archives have a significant size they can benefit of a faster download. 

## Usage

    $ cat <<EOF > S3file
    s3://my-private-bucket/path/to/tarball.tgz
    s3://my-other-bucket/path/to/somethingelse.tgz
    http://my-public-domain.com/tarball.tgz
    https://my-other-public-domain.com/path/theother.tgz
    EOF

    $ pack build my-app --builder heroku/buildpacks:20 --buildpack fagiani/s3-tarball@0.1.1 \
      --env AWS_ACCESS_KEY_ID=AKIA000000000000000 --env AWS_SECRET_ACCESS_KEY=xxxxxxxxxxxxxxxxxx ...

> Alternatively you can use `S3_AWS_ACCESS_KEY_ID` and `S3_AWS_SECRET_ACCESS_KEY` to avoid IAM
> conflicts when using AWS containers to run `pack build`

You probably want to use an [IAM key](http://aws.amazon.com/iam/) with limited
access. This code only requires `s3:GetObject` access to files.

In most cases you'll use this buildpack in conjunction with other buildpacks.

> Please notice that public tarball URLs are also accepted and for that no credentials are required.

## See also

  * [s3-tarball-buildpack](https://github.com/paulhammond/s3-tarball-buildpack)
  * [heroku-buildpack-vendorbinaries](https://github.com/peterkeen/heroku-buildpack-vendorbinaries)
  * [s3simple](https://github.com/paulhammond/s3simple)
  * [Heroku Slug API](https://blog.heroku.com/archives/2013/12/20/programmatically_release_code_to_heroku_using_the_platform_api)

## Contributing

Feel free to contribute by opening a issue or sending a PR.

## Licence

MIT license, see LICENSE.txt for details.
