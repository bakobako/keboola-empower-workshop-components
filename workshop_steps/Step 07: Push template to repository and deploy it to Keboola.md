## Step 7: Push template to repository and deploy it to Keboola
Once the template is ready, you need to push it to your GitHub repository. 
CookieCutter already creates an initial commit, so all you have to do is push it to the repository with :

```
git push
```

Then in GitHub:

* Navigate to the main page of the repository. 
* To the right of the list of files, click Releases.
* At the top of the page, click Draft a new release.
* Create a tag for the release : 0.0.1
* To release click Publish release


It might take up to 5 minutes for the new deployed component to be available in Keboola. You deploy the component image to the
developer portal, and that syncs with the multi-tenant stacks every 5 minutes.


[Next Step](https://github.com/bakobako/keboola-empower-workshop-components/blob/main/workshop_steps/Step%2008%3A%20Develop%20the%20component%20code%20and%20UI.md)