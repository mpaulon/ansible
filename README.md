# Accéder au vault
Le mot de passe du vault est dans `pass` sous le nom `ansible/vault`.

Pour vous faciliter la tache, copiez le dans `.vault_password` à la racine de ce dossier.

Pour avoir un joli diff du vault, exécutez `git config diff.ansible-vault.textconv "ansible-vault view"` dans ce repo