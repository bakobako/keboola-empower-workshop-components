## Step 07: Push template to repository and deploy it to Keboola
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

It can take up to 1 hour for a newly deployed component to be runnable in Keboola. After this hour, any new updates to the 
component can take up to 5 minutes to be available in Keboola. As new components are synced every hour, and new component images 
are synced every 5 minutes.


[Next Step](https://github.com/bakobako/keboola-empower-workshop-components/blob/main/workshop_steps/Step%2008%3A%20Develop%20the%20component%20code%20and%20UI.md)