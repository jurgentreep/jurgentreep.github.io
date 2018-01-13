# jurgentreep.github.io
my own github pages page

## Run docker container
```
docker run --rm -tiv "$PWD:/srv/jekyll" -p 4000:4000 jekyll/jekyll:pages jekyll serve --watch --incremental --force_polling
```