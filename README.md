# announce-code-review

**announce-code-review** is a GitHub Action that automatically posts your pull requests for review at the CodeKeepers Discord Server via our proprietary API. This action performs a series of checks to ensure authenticity before triggering a review request.

## Features

- **Repository Privacy Check:**  
  Ensures that the repository is public.
- **Badge Verification:**  
  Checks that the repository's `README.md` contains the CodeKeepers badge (the "Join CodeKeepers" image) and verifies that it was committed by **Vulps22**.
- **Review Request Submission:**  
  If all checks pass, it sends a POST request to the CodeKeepers API endpoint with the pull request URL and repository owner details.

## Usage

- Join the [CodeKeepers Discord Server](https://discord.gg/uhvQpVmxeK) if you haven't done so already
- Complete the verification process by getting the Github Verified linked role and registering your project
- use `/secret generate` to generate a secret for your project
- Create a workflow file (e.g., `.github/workflows/review-request.yml`) in your repository with the following content:

```yaml
name: CodeKeepers Review Request

on:
  pull_request:
    types: [opened, reopened, synchronize]

jobs:
  review_request:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run announce-code-review Action
        uses: Vulps22/announce-code-review@v1.0.0
        with:
          api_token: ${{ secrets.API_TOKEN }}
          pr_url: ${{ github.event.pull_request.html_url }}
```

> **Note:**  
> - Make sure you add your API token in your repository secrets as `API_TOKEN`.

- Once your project meets all the requirements for approval, Vulps22 (Discord: vulps23) will sign your project with this image, Signifying your project is ready for review:
  [<img src="https://vulps.co.uk/raw/img/codekeepers-text.png" alt="Join CodeKeepers" height="75"/>](https://discord.gg/uhvQpVmxeK)

## Inputs

| Input       | Description                                                        | Required |
|-------------|--------------------------------------------------------------------|----------|
| `api_token` | API token for authenticating with the CodeKeepers endpoint         | Yes      |
| `pr_url`    | The pull request URL (e.g., `${{ github.event.pull_request.html_url }}`) | Yes      |

## What the Action Does

1. **Repository Privacy Check:**  
   Verifies that the repository is public. If it is private, the action exits.
2. **Badge Check:**  
   Searches the `README.md` for the "Join CodeKeepers" badge.
3. **Commit Verification:**  
   Uses `git blame` on the line containing the badge to confirm that it was committed by **Vulps22**.
4. **Review Request Submission:**  
   Sends a POST request to `https://vulps.co.uk/codekeepers/api/review-request` with a JSON payload that includes the pull request URL and the repository owner's username (fetched from the GitHub event context).
5. The CodeKeepers Discord bot will post your PR in the #pull-requests channel on discord so that others can review your code 🙂

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to open an issue or submit a pull request.

## Disclaimer

This GitHub Action is designed to work with the CodeKeepers Discord Bot API. By using this action, you agree to the terms outlined in the Apache License 2.0 and acknowledge that the API usage is governed by CodeKeepers' proprietary terms.
