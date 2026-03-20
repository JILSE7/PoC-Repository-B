PoC Repository B


## Registering a new repository

First we need to add the submodule PoC Repository A

git submodule add https://github.com/JILSE7/PoC-Repository-A.git poc-submodule 
// it inmediatly downloads the repository in the poc-submodule folder

Then we need to update the submodule and sparse-checkout to only get the domain and docs folders

cd poc-submodule 
git sparse-checkout init --cone
git sparse-checkout set domain docs // it tells git to only get the domain and docs folders


## Updating the submodule Commit and Push to Repository A (The Submodule)
First, do the changes in the submodule folder, then you must navigate into the submodule folder to save your work.

bash
# 1. Go into the submodule
cd poc-submodule
# 2. Add and commit your changes
git add .
git commit -m "feat: service entity added"
# 3. Push the changes to GitHub (PoC-Repository-A)
git push origin main // it updates the submodule in PoC Repository A

Update Repository B to Point to the New Version
Repository B still thinks the submodule is at the old version. You need to tell Repository B that the submodule has moved forward.

bash
# 1. Go back up to the main Repository B folder
cd ..
# 2. Tell Repo B to track the new submodule commit
git add poc-submodule // it is the name of the submodule folder
git commit -m "Moved poc-submodule to the latest commit" // it is the message of the commit
# 3. Push the pointer update to GitHub (PoC-Repository-B)
git push // it updates the submodule in PoC Repository B


## Pulling the newest changes from Repository A into your submodule inside Repository B
To pull the newest changes from Repository A into your submodule inside Repository B, here is the two-step process:

From the root folder of PoC-Repository-B, run the following command:

bash
git submodule update --remote poc-submodule 
// This command commands Git to go into the poc-submodule folder, download the latest commits from GitHub for Repository A, and update your files.

Save the New "Pointer" in Repository B
Now your submodule has the new code, but Repository B needs to remember that we are using this newer version. If you run git status in Repo B right now, it will say modified: repo-a-links (new commits).

You just need to commit this updated pointer:

bash
git add poc-submodule // it stages the submodule in PoC Repository B
git commit -m "Pulled the latest changes from PoC-Repository-A" // it commits the submodule pointer in PoC Repository B
git push // it pushes the submodule pointer in PoC Repository B


## Unlinking the submodule from Repository B
Step 1: Unregister the Submodule
This step removes the submodule's configuration from your local .git/config file so Git stops tracking it.

bash
git submodule deinit -f -- poc-submodule // it unregisters the submodule in PoC Repository B

Step 2: Delete the Folder and Remove from Git Cache
This deletes the physical repo-a-links folder from your workspace and deletes the reference in the .gitmodules file.

bash
git rm -f poc-submodule // it removes the submodule folder from PoC Repository B

Step 3: Delete the Hidden Git Data
Even after removing the folder, Git keeps the downloaded files cached deep inside your hidden .git folder (just in case you want to add it back later). It is highly recommended to delete this, otherwise, if you try to add a submodule with the exact same name later, Git will throw an error.

bash
rm -rf .git/modules/poc-submodule // it removes the hidden git data of the submodule in PoC Repository B

Step 4: Commit the Removal
Finally, commit these changes so Repository B remembers that the submodule is gone forever.

bash
git commit -m "Completely removed the poc-submodule" // it commits the removal of the submodule in PoC Repository B
git push // it pushes the removal of the submodule in PoC Repository B




