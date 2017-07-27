[ReDoc](https://github.com/Rebilly/ReDoc) has a JavaScript hack for hosting multiple APIs from one HTML page, but that doesn't really work out so well. Even then, how do you get your [Open API] files from the repository they (should) live in, to somewhere that's publically accessable over HTTP?

LETS MAKE SOME YAML FILES, and turn Open API specs into multiple beautiful ReDoc pages in a static site.

## How?

Each repository will have a folder that contains all of the Open API specification files (probably `docs/` for now), and an "entry file" inside that (probably `api.yml`).

**Commands**

- `rake` will clone all of the repos, copy out the specification folder, then generate a `public/index.html` with a list of APIs in it
- `rake build` will skip cloning and just build HTML out again
- `rake serve` will server HTML on [`localhost:8000`](http://localhost:8000)

## Adding a Repo

The file `data/apis.yml` keeps track of all the Open API specification files, and a new repository might look like this:

```
service-a:
  name: Service A
  repo: "git@github.com:org/repo.git"
  branch: master
  spec_dir: docs/
  entry_file: api.yml
```

Make sure wherever this is running (Jenkins or whatever) has access to the Github repo, so maybe make a bot user and invite it.

That's it. Shove the whole thing up on hosting somewhere and build from `public/` and you're done. Or S3 the HTML whatever. This built in ruby server is not going to last very long so don't use that.

## "What about Postman?"

Postman is an amazing HTTP client, and bookmark manager for HTTP requests. Sadly it doesn't really handle any sort of documentation, nor does it support contract definition or data modeling in any way.

Data modeling is useful for explaining what values `status` could contain, and what do they mean? What are the minimum or maximum values? What format should that date field be?

Looking at raw data and guessing leads to mistakes. Specifying output gives us this human documentation, and soon we'll have documentation for computers too (JSON Schema).

If you have a Postman collection already you can throw it through [API Transformer](https://apitransformer.com/) and select `Swagger v2.0 (YAML)`. It'll take a bit of tweaking, but you could have a basic start to your amazing new Open API-based documentation in no time.

[Open API]: https://swagger.io/specification/
