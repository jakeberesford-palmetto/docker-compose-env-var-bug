# Bug report - interpolated env vars do not work in env_file

In many of our applications, we use interpolation of env vars in the env_file
to read environment variables set on the host machine. This allows our developers
to set common environment variables once, and make them available across different
projects. When upgrading to docker-compose v2.x we noticed that these enviornment variables
no longer work. I have not seen release notes or documentation around this specific issue.

This repository is the minimum code required to replicate the issue. 
A simple bash container which runs `printenv`

## To replicate the issue

1. Add the $SHELL_BAR env variable to your shell. For zsh:
`echo "\nexport SHELL_FOO='foo'" >> ~/.zshrc`

Source your shell to apply changes `. ~/.zshrc`

2. Run the container, pipe to grep to check for the env var we care about
`docker-compose run env_var_test | grep 'FOO_VAR'`

expected output, as seen in docker-compose 1.29.2

```bash
Creating env-var-compose_env_var_test_run ... done
FOO_VAR=foo
```

3. Run the container with latest docker-compose v2
`docker compose run env_var_test | grep 'FOO_VAR'`

New output does not have the interpolated env var

```bash
Creating env-var-compose_env_var_test_run ... done
FOO_VAR=
```