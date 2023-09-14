GitHub Action Pinning & Its Security Implications

- Context: GitHub Actions is a powerful tool for automating software development tasks. A common best practice is "action pinning," where users pin actions to a specific commit SHA to prevent unwanted changes and potential supply chain attacks.
- The Pitfall: However, there's a security risk with certain "unpinnable actions" which, even when pinned, can introduce new code into a user's pipeline. This allows attackers to execute malicious code.

- How Attackers Exploit the Gap:
    - Some actions, despite being pinned to a specific SHA, pull in external resources that can change over time. Examples include mutable Docker container images, or other third-party actions that haven't been securely pinned.
    - For Docker containers, both pulling images from a registry and supplying a Dockerfile to the runner can be exploited.
    - Composite actions, which allow the writing of Bash commands directly, can break pinning security if they reference another unpinned action or pull in outside resources.
    - JavaScript actions can also be vulnerable if they fetch outside resources at runtime without adequate checks.

- Research Findings: 
    - Out of the top 1,000 starred actions on GitHub Marketplace, 32% were found to be unpinnable.
    - Analyzing top-starred public open-source projects revealed that 67% of them used unpinnable actions, even when they thought they were pinning them securely.

- Recommendations:
    1. Ensure visibility into all actions and their dependencies to identify unpinnable actions.
    2. Always pin actions to specific SHAs.
    3. Implement strict access controls in CI workflows to minimize potential damage from attacks.
    4. Consider forking and securing unpinnable actions for safer usage.
    5. If creating actions, ensure they are truly pinnable by locking all dependencies and external resources.

- Conclusion: While action pinning is a valuable security measure, it's not infallible. Both public and private repositories using GitHub Actions should be aware of the risks and take steps to mitigate them.
