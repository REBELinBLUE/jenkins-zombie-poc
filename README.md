# Proof of concept for defunct process on agent

1. Start up the controller running by `docker compose up -d controller`
2. Visit http://localhost:8080
3. Click on static-agent http://localhost:8080/computer/static%2Dagent/ and copy the secret, place it in `secrets/static-agent.txt`
4. Click on static-agent-with-tini http://localhost:8080/computer/static%2Dagent%2Dwith%2Dtini/ and copy the secret, place it in `secrets/static-agent-with-tini.txt`
5. Start up the agents by running `docker compose up -d`
6. Monitor the agent without tini by running `docker compose exec agent watch ps aux`
7. Kick off the `test job for normal agent` http://localhost:8080/job/test%20job%20for%20normal%20agent/
8. Watch the process in the output from 6, when the job finishes notice you have a zombie process for `sh` (`stat` is `Z` and `command` is `[sh] defunct`). You can now ctrl+c
9. Monitor the agent with tini by running `docker compose exec agent-with-tini watch ps aux`
10. Kick off the `test job for agent with tini` http://localhost:8080/job/test%20job%20for%20agent%20with%20tini/
11. Watch the process in the output from 9, when the job finishes notice you do not have a zombie process for `sh`. You can now ctrl+c

You can add `init: true` to the container definition in `docker-compose.yaml` or pass `--init` when running a container with `docker run` to cause Docker to wrap the entrypoint in tini itself, but this only works with Docker; this is not possible when running the agent container as a Kubernetes pod. The Jenkins controller explicitly wraps the entrypoint in it's `Dockerfile` but the inbound-agent doesn't.
