### GitHub Actions?
- Automation tool
- Similar to Travis-CI, GitLab CI/CD, Jenkins
- Run jobs any time code is changed
- the jobs automate sudden tasks

common uses for github actions
- handle the Deployment
- handlethe code linting
- run unit tests

 Trigger (e.g. push) -> Job (unit tests) -> Result/Output (Success/fail)

 Trigger can be anything that happens to my project on github.
 Set up jobs that run when trigger is hit.

1. Create folder on root directory. /.github/workflow/
2. Create file in workflow /.github/workflow/checks.yml
    ```yaml
    name: the name will appear in github action.
    on: Trigger. any pushed our project, run the jobs.

    jobs:
      test-lint:
        name: It's going to appear in github action interface.
        runs-on: The runner, run jobs (operating system)
        steps:
            - name: Human readable name.
              uses: uses allows to use another pre-made action that's provided in the Github action repository.
              with: pass the parameters.
                username: ${{ secrets.DOCKERHUB_USER }}
                password: ${{ secrets.DOCKERHUB_TOKEN }}
            - name:
              uses:
            - name:
              run: if name is Test running test command.
            - name:
              run:

    ```
3. I can check the GitHub Actions what I added at Actions of GitHub repository.
There's another actions was added automatically.