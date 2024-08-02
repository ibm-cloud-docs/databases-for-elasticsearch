# Databases for Elasticsearch documentation

This repo contains documentation for the Databases for Elasticsearch services.

## Contacts for this repo

- Andrea Lang: ANDREAL, andreal@de.ibm.com

## Creating content

Follow these steps to add to the deployment guide docs.

:information_source: **Tip:** If you'd rather give feedback about the documentation, create an [issue](https://github.ibm.com/compose/compose/issues/new?assignees=&labels=guild-devcom%2C+icd&template=icd-docs-issue.md).

### Before you begin
Set up your local development environment.

- Visual Studio Code is the recommended editor. For more information, see [Choose an editor](https://test.cloud.ibm.com/docs-internal/writing?topic=writing-setting-up-your-markdown-environment#choose-an-editor).
- You can install a Markdown linter in Visual Studio Code that works with IBM Cloud docs. For more information, see [Integrating the Markdown linter in VS Code](https://test.cloud.ibm.com/docs-internal/writing?topic=writing-markdown-linter-vscode).

### Drafting content

All content starts from the `source` branch.

1.  Clone this repo if you have write privileges. Otherwise, fork the repo.
1.  Create a working branch from the `source` branch.
1.  Make your changes to the Markdown content.

    - Cloud docs uses Markdown with a few custom extensions to source the docs. For tips about how to structure and style your docs with IBM Cloud Markdown, see [Quick tips for authoring in IBM Cloud docs](https://test.cloud.ibm.com/docs-internal/writing?topic=writing-solution-guides#solution-guides-include-quick-tips) in "Creating solution, deployment, and migration guides".
    - Cloud docs also supports controlling content with tagging. For example, content within the staging code tags is not displayed to the public. For more information, see [Making updates to your docs](https://test.cloud.ibm.com/docs-internal/writing?topic=writing-update-docs).

1.  Commit your updates.
1.  Create a pull request from your branch or fork to `source`.

    1.  A Jenkins job runs that commits content to the `draft` and `review` branches and opens a pull request to the `publish` branch.
    1.  After a few minutes, you can see your changes in the IBM Cloud docs framework. Changes to `draft` are available at the internal `/docs-draft/` location (https://test.cloud.ibm.com/docs-draft/databases-for-elasticsearch). Changes to `review` are available at the pre-production `/docs/` location (https://test.cloud.ibm.com/docs/databases-for-elasticsearch).

:information_source: **Tip:** Content that is tagged with draft, review, or staging, or feature tags are built and promoted only to the internal location and is not included in the pull request to the `publish` branch for production.

## Publishing to production

When content is committed to the `source` branch, a pull request is created from a branch called `next-prod-push` to the `publish` branch. So, changes can't go into the branch until they are reviewed, edited, and approved by a one of the doc maintainers.

The documentation team reviews your PR and might request changes.

- If the changes are relatively straightforward and self-contained, such as a corrected typographical error or a rewritten sentence, we will approve and merge them after issues are addressed.
- If the changes are more extensive, such as a significant rewrite or entirely new content, the documentation team might need to make revisions for editorial or style reasons.⁠ In this case, we might open a new PR against your branch with our proposed revisions.⁠ You can then review these revisions and incorporate the changes into your branch.⁠ After the documentation team is satisfied with the proposed changes, we merge your PR.⁠
When changes are merged to the `production` branch, they are built and published to production at https://cloud.ibm.com/docs/databases-for-elasticsearch.

### Guidelines for merging a PR

As someone with merge responsibilities, follow these guidelines and practices to make sure that changes are released consistently.


- Make sure that the merge commit message is clear and specific.

    :exclamation: **Important:** Do not expose IBM Confidential information in your commit message to `publish`. Commits made to the `publish` branch become public record. When you merge to the `publish` branch, the source is mirrored in a public GitHub repo at https://github.com/ibm-cloud-docs/databases-for-elasticsearch so that customers can view and contribute to the source.
- Squash and merge

    - Use the **Squash and merge** option when you merge a PR. Status checks prevent the merge if the squash and merge method is not used. For more information, see [Squashing your merge commits](https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/about-merge-methods-on-github#squashing-your-merge-commits).


## Monitoring

After a build is triggered by a commit or merge, you can monitor progress.

### Monitoring builds

The Slack channel [#docs-databases-for-elasticsearch](https://ibm.enterprise.slack.com/archives/C02D2K5JN10) displays information about builds.

### Monitoring content quality

You can monitor your content quality on the Content Quality Dashboard (CQD): https://cops.console.test.cloud.ibm.com/docs-quality-dashboard. The CQD for docs content is updated daily For more information, see https://test.cloud.ibm.com/docs-internal/writing?topic=writing-cqd.

## More information

- Learn about the suggested content for each type of solution docs at https://test.cloud.ibm.com/docs-internal/writing?topic=writing-writing-solution.

## Output

The output from the `draft` branch is available on the staging server: 

- [Staging Docs](https://test.cloud.ibm.com/docs/services/databases-for-elasticsearch)

The output from the `publish` branch is mirrored on [github.com](https://github.com/ibm-cloud-docs/databases-for-elasticsearch) and available in production:
- [Production Docs](https://cloud.ibm.com/docs/services/databases-for-elasticsearch)

## Resources

- [Getting started: writing docs for IBM Cloud](https://test.cloud.ibm.com/docs/developing/writing?topic=writing-get-started-onboarding)
- [IBM Cloud doc build](https://test.cloud.ibm.com/docs/developing/writing?topic=writing-get-start-docbuilds)

For general questions about IBM Cloud Databases, see the [icd-questions channel](https://ibm-cloudplatform.slack.com/messages/C534XRCF3/).

## Blog Submissions

To submit a blog, go [here](https://w3.ibm.com/w3publisher/ibm-cloud-blog/submit-a-post). Ian Smalley handles everything blog-related, so any updates go through him. Use the same form to update, just make a note that it's an update, not a new post.
