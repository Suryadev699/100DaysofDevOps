## Day 29: Manage Git Pull Requests

Today, we will learn how to manage Git pull requests using the command line interface (CLI). Pull requests are a way to propose changes to a codebase and collaborate with other developers. We will cover the following topics:

1. **Creating a Pull Request**: How to create a pull request from a feature branch to the main branch.
2. **Reviewing a Pull Request**: How to review the changes in a pull request and leave comments.
3. **Merging a Pull Request**: How to merge a pull request into the main branch after approval.
4. **Closing a Pull Request**: How to close a pull request without merging it.
5. **Using GitHub CLI**: How to use the GitHub CLI tool to manage pull requests directly from the terminal.
### 1. Creating a Pull Request
To create a pull request, you can use the GitHub CLI tool. First, ensure you have the GitHub CLI installed and authenticated. Then, navigate to your repository and run the following command:

```bash
gh pr create --base main --head feature-branch --title "My Feature" --body "This pull request adds a new feature."
```
This command creates a pull request from `feature-branch` to `main` with the specified title and body.
### 2. Reviewing a Pull Request
To review a pull request, you can use the following command to view the changes:
```bash
gh pr view <pr-number> --web
```
This command opens the pull request in your web browser, where you can review the changes and leave comments.
### 3. Merging a Pull Request
Once the pull request has been reviewed and approved, you can merge it using the following command:
```bash
gh pr merge <pr-number> --merge
```
This command merges the pull request into the main branch.
### 4. Closing a Pull Request
If you decide not to merge a pull request, you can close it using the following command:
```bash
gh pr close <pr-number>
```
This command closes the pull request without merging it.
### 5. Using GitHub CLI
The GitHub CLI tool provides a powerful way to manage pull requests directly from the terminal. You can list all open pull requests with:
```bash
gh pr list
```
You can also check the status of a specific pull request with:
```bash
gh pr status
```
By mastering these commands, you can efficiently manage pull requests and collaborate with your team using the command line interface. Happy coding!