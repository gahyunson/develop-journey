### GitHub Actions?
- Automation tool, performs a complex but frequently repeated task.
- Similar to Travis-CI, GitLab CI/CD (Continuous Integration and Continuous Delivery), Jenkins
- A workflow == a configurable automated process.
- Can have multiple workflows.
- Workflows are defined in the `.github/workflows`

Job?
- A set of steps in a workflow.
- You can share data from one step to another if each step is executed on the same runner.
e.g. A step that builds a application - a step that tests a application that was build.
- jobs have no dependencies: run in parallel.
- A job takes a dependency on another job: it waits for the dependents job to complete.

Runners?
- A server that runs workflows.
- Each runner can run a single job at a time.


common uses for github actions
- handle the Deployment
- handlethe code linting
- run unit tests

 Trigger (e.g. push) -> Job (unit tests) -> Result/Output (Success/fail)

 Trigger can be anything that happens to my project on github.
 Set up jobs that run when trigger is hit.

1. Create folder on root directory. /.github/workflows/
2. Create file in workflow /.github/workflows/checks.yml
    ```yml
    ---
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