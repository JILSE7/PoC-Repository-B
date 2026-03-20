PoC Repository B


Registering a new repository

First we need to add the submodule PoC Repository A

git submodule add https://github.com/JILSE7/PoC-Repository-A.git poc-submodule 
// it inmediatly downloads the repository in the poc-submodule folder

Then we need to update the submodule and sparse-checkout to only get the domain and docs folders

cd poc-submodule 
git sparse-checkout init --cone
git sparse-checkout set domain docs // it tells git to only get the domain and docs folders



