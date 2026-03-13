# Databases for Elasticsearch documentation

This repo contains documentation for the Databases for Elasticsearch services.

## Contacts for this repo

- Andrea Lang: ANDREAL, andreal@de.ibm.com

## Creating content

Follow these steps to add to the deployment guide docs.

:information_source: **Tip:** If you'd rather give feedback about the documentation, create an 

### Before you begin
Set up your local development environment.

- Visual Studio Code is the recommended editor. 
- You can install a Markdown linter in Visual Studio Code that works with IBM Cloud docs. 
### Drafting content

All content starts from the `source` branch.

1.  Clone this repo if you have write privileges. Otherwise, fork the repo.
1.  Create a working branch from the `source` branch.
1.  Make your changes to the Markdown content.

    

1.  Commit your updates.
1.  Create a pull request from your branch or fork to `source`.

    1.  A Jenkins job runs that commits content to the `draft` and `review` branches and opens a pull request to the `publish` branch.
    1.  After a few minutes, you can see your changes in the IBM Cloud docs framework. 

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

## Monitoring

After a build is triggered by a commit or merge, you can monitor progress.

### Monitoring builds

The Slack channel [#docs-databases-for-elasticsearch](https://ibm.enterprise.slack.com/archives/C02D2K5JN10) displays information about builds.

### Monitoring content quality

You can monitor your content quality on the Content Quality Dashboard (CQD): 

## More information

- Learn about the suggested content for each type of solution docs 

## Output

The output from the `draft` branch is available on the staging server: 



The output from the `publish` branch is mirrored on [github.com](https://github.com/ibm-cloud-docs/databases-for-elasticsearch) and available in production:
- [Production Docs](https://cloud.ibm.com/docs/services/databases-for-elasticsearch)

## Resources



For general questions about IBM Cloud Databases, see the [icd-questions channel](https://ibm-cloudplatform.slack.com/messages/C534XRCF3/).

## Blog Submissions

 Ian Smalley handles everything blog-related, so any updates go through him. Use the same form to update, just make a note that it's an update, not a new post.
