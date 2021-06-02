## <font color='red'> 2.2 Variables </font>
Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. You can define these variables in your playbooks, in your inventory, in re-usable files or roles, or at the command line. You can also create variables during a playbook run by registering the return value or values of a task as a new variable.

After you create variables, either by defining them in a file, passing them at the command line, or registering the return value or values of a task as a new variable, you can use those variables in module arguments, in conditional “when” statements, in templates, and in loops. The ansible-examples github repository contains many examples of using variables in Ansible.

  > For further information: https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html

In this lab we're going to:
* create a simple GitHub Action



---

#### <font color='red'>Custom Variables</font>
You can set up continuous integration for your project using a workflow template that matches the language and tooling you want to use.

List of GitHub Actions:
* github actions demo
* hello world
* different shells


So lets dive in the deep end and create a workflow thats shows whats happening behind the scenes.

  > GitHub Actions repository:  http://github.com/jporeilly/GitHub-Actions.git

**github actions demo**
* create a new branch for the workflow: github-actions-demo
* then add this workflow - github-actions-demo.yaml - to the github-actions-demo/.github/workflows/  directory:

```
name: GitHub Actions Demo
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v2
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```

* This will trigger a push event.
* On GitHub, navigate to the main page of the repository.
* Under your repository name, click Actions.
* In the left sidebar, click the workflow you want to see.
* From the list of workflow runs, click the name of the run you want to see.
* Under Jobs, click the Explore-GitHub-Actions job.

**hello world**
* create a new branch for the workflow: hello-world
* then add this workflow - hello-world.yaml - to the hello-world/.github/workflows/  directory:

```
name: 01 Hello World 
on: [push]
jobs:
  run-shell-command:
    runs-on: ubuntu-latest
    steps: 
      - name: Echo a string
        run: echo "Hello World"
      - name: Multiline script 
        run: |
           node -v 
           npm -v
```
* This will trigger a push event.
* On GitHub, navigate to the main page of the repository.
* Under your repository name, click Actions.


**different shells**
* create a new branch for the workflow: different-shells
* then add this workflow - different-shells.yaml - to the different-shells/.github/workflows/  directory:

```
name: 02 Different Shells 
on: [push]
jobs:
  run-shell-command:
    runs-on: ubuntu-latest
    steps: 
      - name: echo a string
        run: echo "Hello World"
      - name: multiline script 
        run: |
           node -v 
           npm -v
      - name: python Command 
        run: |
          import platform 
          print
          (platform.processor())
        shell: python
  run-windows-command:
    runs-on: windows-latest
    needs: ["run-shell-command"]
    steps:
      - name: Directory PowerShell
        run: Get-Location 
      - name: Directory Bash 
        run: pwd 
        shell: bash 
```
* This will trigger a push event.
* On GitHub, navigate to the main page of the repository.
* Under your repository name, click Actions.


**hello world - Marketplace**
* create a new branch for the workflow: marketplace
* click on the Actions tab
* click on the New Workflow
* select 'Simple Workflow'
* deploy to main

---