# What is Redis?
Redis is an open-source, networked, in-memory, key-value data store with optional durability. It is written in ANSI C. The development of Redis has been sponsored by Pivotal since May 2013; before that, it was sponsored by VMware. According to the monthly ranking by DB-Engines.com, Redis is the most popular key-value store. The name Redis means REmote DIctionary Server.

> [wikipedia.org/wiki/Redis](https://en.wikipedia.org/wiki/Redis)

# How to use this image

## start a redis instance
    docker run --name some-redis -d redis

This image includes `EXPOSE 6379` (the redis port), so standard container linking will make it automatically available to the linked containers (as the following examples illustrate).

## start with persistent storage

    docker run --name some-redis -d redis redis-server --appendonly yes

If persistence is enabled, data is stored in the `VOLUME /data`, which can be used with `--volumes-from some-volume-container` or `-v /docker/host/dir:/data` (see [docs.docker volumes](http://docs.docker.com/userguide/dockervolumes/)).

For more about Redis Persistence, see [http://redis.io/topics/persistence](http://redis.io/topics/persistence).

## connect to it from an application
    docker run --name some-app --link some-redis:redis -d application-that-uses-redis

## ... or via `redis-cli`
    docker run -it --link some-redis:redis --rm redis sh -c 'exec redis-cli -h "$REDIS_PORT_6379_TCP_ADDR" -p "$REDIS_PORT_6379_TCP_PORT"'

## Additionally, If you want to use your own redis.conf ...

You can create your own Dockerfile that adds a redis.conf from the context into /data/, like so.

    FROM redis
    redis.conf /data/
    CMD [ "redis-server", "/data/redis.conf" ]

Alternatively, you can specify something along the same lines with `docker run` options.

    ocker run --volumes-from datacontainer --name myredis redis

Using this method means that there is no need for you to have a Dockerfile for your redis container.



# Issues and Contributing

We are always thrilled to receive pull requests, and do our best to process them as fast as possible. Not sure if that typo is worth a pull request? Do it! We will appreciate it.

If your pull request is not accepted on the first try, don't be discouraged! If there's a problem with the implementation, hopefully you received feedback on what to improve.

We recommend discussing your plans through a [GitHub issue](https://github.com/docker-library/redis/issues) before starting to code - especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give feedback on your design, and maybe point out if someone else is working on the same thing.

Any significant improvement should be documented as a GitHub issue before anybody starts working on it. Please take a moment to check that an issue doesn't already exist documenting your bug report or improvement proposal. If it does, it never hurts to add a quick "+1" or "I have this problem too". This will help prioritize the most common problems and requests.

## Conventions

Fork the repository and make changes on your fork in a feature branch.

Update this documentation when creating or modifying features. Test your documentation changes for clarity, concision, and correctness.

Pull requests descriptions should be as clear as possible and include a reference to all the issues that they address.

Commit messages should start with a capitalized and short summary (max. 50 chars) written in the imperative, followed by an optional, more detailed explanatory text which is separated from the summary by an empty line.

Code review comments may be added to your pull request. Discuss, then make the suggested modifications and force push amended commits to your feature branch. Be sure to post a comment after pushing. The changed commits will show up in the pull request automatically, but the reviewers will not be notified unless you comment.

Before the pull request is merged, make sure that you squash your commits into logical units of work using `git rebase -i` and `git push -f`. Include documentation changes in the same commit so that a revert would remove all traces of the feature or fix.

Commits that fix or close an issue should include a reference like Closes #XXXX or Fixes #XXXX, which will automatically close the issue when merged.