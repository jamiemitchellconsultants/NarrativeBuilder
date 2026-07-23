# Start here: set up your repository and coding agent

This guide is for learners who have not used GitHub or an AI coding agent before. When you finish,
you will have:

- a GitHub repository named `MyNarrative`;
- an optional local copy of that repository on your computer;
- one coding agent connected to the correct repository;
- a clear understanding of whether the agent is working locally or in the cloud; and
- a safe first task that proves the setup works without changing files.

You do not need every agent mentioned below. Pick one route and return to the alternatives later if
they become useful. Product availability, plan requirements, and interface labels can change; use
the linked official documentation if what you see differs from this guide.

## 1. Create `MyNarrative` on GitHub

You need a free [GitHub account](https://github.com/signup).

1. Sign in to GitHub.
2. Open [Create a new repository](https://github.com/new).
3. Choose your personal account or organisation as the owner.
4. Enter `MyNarrative` as the repository name.
5. Add a description such as `My guided Project Narrative implementation`.
6. Choose **Private** while learning unless you deliberately want it public.
7. Select **Add a README file**.
8. Leave `.gitignore` and licence unselected; the prompts add them deliberately.
9. Select **Create repository**.

The GitHub repository is the remote, shared copy. At this point it contains only `README.md`.
GitHub also provides an official
[repository-creation walkthrough](https://docs.github.com/en/get-started/start-your-journey/creating-a-repository-from-scratch).

## 2. Choose local or cloud work

| | Local agent | Cloud agent |
|---|---|---|
| Where it runs | On your computer | In a provider-hosted environment |
| How it gets the code | You select a cloned folder | You authorise a GitHub repository |
| Sees uncommitted local files | Yes | No |
| Uses tools on your computer | Yes | No |
| How changes appear | In the local working tree | Usually on a remote branch or pull request |

The recommended first route is a local desktop agent. It makes the relationship between files, Git
changes, tests, commits, and pushes easier to see.

## 3. Clone the repository locally

Cloning creates a working copy on your computer.

### Recommended: GitHub Desktop

[Download GitHub Desktop](https://desktop.github.com/download/), install it, and sign in to the
account that owns `MyNarrative`.

1. Open `MyNarrative` on GitHub.
2. Select **Code**, then **Open with GitHub Desktop**.
3. Choose a local folder and select **Clone**.
4. In GitHub Desktop, select **Repository → Show in Finder** on macOS or
   **Repository → Show in Explorer** on Windows.
5. Confirm the folder contains `README.md`.

See GitHub's
[cloning guide](https://docs.github.com/en/desktop/adding-and-cloning-repositories/cloning-a-repository-from-github-to-github-desktop)
if those controls have moved.

### Command-line alternative

If you already use a terminal and Git:

```bash
git clone https://github.com/YOUR-GITHUB-NAME/MyNarrative.git
cd MyNarrative
```

Replace `YOUR-GITHUB-NAME` with the owner of the repository.

`NarrativeBuilder` contains the learning prompts. `MyNarrative` is the separate repository where
your agent builds the processor. Do not select the builder repository when starting the coding
task.

## 4. Connect one coding agent

Choose one route. Git and the checked-in files remain the shared source of truth if you switch
agents later.

### Codex on desktop

1. Install the current ChatGPT desktop application from
   [OpenAI's Codex getting-started page](https://openai.com/codex/get-started/).
2. Open Codex and add a project.
3. Select the locally cloned `MyNarrative` folder.
4. Choose a local environment and keep the normal approval protections enabled.

### Claude Code Desktop

1. [Download Claude Desktop](https://claude.com/download), install it, and sign in.
2. Open **Code**, select **Local**, and choose the `MyNarrative` folder.
3. Keep the normal permission prompts enabled.

See the [Claude Code desktop quickstart](https://code.claude.com/docs/en/desktop-quickstart) if Git
or the Code tab is unavailable.

### GitHub Copilot cloud agent

1. Confirm your GitHub account has an eligible Copilot plan.
2. Open `MyNarrative` on GitHub.
3. Open the repository's **Agents** tab or
   [github.com/copilot/agents](https://github.com/copilot/agents).
4. Select `MyNarrative` and submit a small task.
5. Review the session log, branch, and pull request the agent creates.

Organisation repositories may require an administrator to enable Copilot agents. See GitHub's
[Copilot coding agent overview](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/about-coding-agent).

For another product, either open the cloned folder for local work or authorise only `MyNarrative`
for cloud work. Never paste a password, personal access token, or `.env` content into a prompt.

## 5. Prove the connection safely

Give the agent this read-only task:

```text
Read this repository and list the files you can see. Confirm the repository name and current
branch. Do not create, edit, delete, commit, push, install, or download anything.
```

A correct result identifies `MyNarrative` and `README.md`. Stop and fix the setup if the agent
reports `NarrativeBuilder`, cannot see the README, requests unrelated repository access, or proposes
changes despite the read-only instruction.

## 6. Begin the learning sequence

Keep NarrativeBuilder open so you can copy one prompt at a time.

1. Give the agent the complete [reusable contract](prompts/00-reusable-contract.md).
2. Wait for it to acknowledge the contract.
3. Give it [Prompt 1](prompts/01-project-scaffold-and-configuration.md).
4. Review the diff and acceptance checks before continuing.
5. Continue in the order listed in [README.md](README.md).

Do not submit every prompt at once. Each stage deliberately limits scope so you can understand the
new behavior and catch errors before more code depends on it.

When a prompt says **commit locally but do not push**, a local agent may create a commit but must not
publish it. A cloud agent normally needs a remote branch; review that branch and its pull request
instead.

## 7. Git words used by the guide

- **Repository:** the project and its version history.
- **Remote:** the shared repository hosted by GitHub.
- **Clone:** a local working copy of the remote.
- **Branch:** an isolated line of work.
- **Working tree:** the files currently visible in a local clone.
- **Commit:** a named snapshot in repository history.
- **Push:** publish local commits to a remote.
- **Pull request:** a GitHub review proposing that one branch be merged into another.
- **Diff:** the exact lines added, removed, or changed.

The coding agent can perform these actions, but you remain responsible for reviewing changes and
deciding what is published or merged.

## 8. Work safely

- Start with a private learning repository.
- Give the agent access only to `MyNarrative`.
- Keep approval and sandbox protections enabled.
- Never commit credentials, tokens, personal data, or production pull-request payloads.
- Use branches for work that will be pushed.
- Read the diff before committing or merging.
- Require the checks named in each prompt.
- Do not approve a destructive command you do not understand.
- Do not merge a pull request merely because an agent created it.

## 9. Common setup problems

### The agent cannot see `README.md`

The wrong folder or repository is selected. Choose `MyNarrative`, not its parent and not
`NarrativeBuilder`.

### A local agent can edit but cannot push

Folder access and GitHub authentication are separate. Sign in through GitHub Desktop, GitHub CLI,
or the agent's supported integration. Do not send credentials through chat.

### A cloud agent cannot see a local change

Cloud agents start from GitHub branches. Commit and push the prerequisite change or continue the
task locally.

### A command works in Terminal but not in the desktop agent

Desktop apps may use a different environment or executable search path. Check the product's local
environment settings instead of repeatedly approving the same failing command.
