# jurgentreep.github.io

My blog.

## Run docker container

Works on my machine command. ¯\\_(ツ)_/¯

```bash
docker run --rm -tiv "<current_path>:/srv/jekyll" -p 4000:4000 jekyll/jekyll:pages jekyll serve --watch --incremental --force_polling
```

Or in WSL2

```
docker run --rm -tiv $PWD:/srv/jekyll -p 4000:4000 jekyll/jekyll:pages jekyll serve --watch
```