# Accéder au vault
Le mot de passe du vault est dans `pass` sous le nom `ansible/vault`.

Pour vous faciliter la tache, copiez le dans `.vault_password` à la racine de ce dossier.

Pour avoir un joli diff du vault, exécutez `git config diff.ansible-vault.textconv "ansible-vault view"` dans ce repo

# DNSSEC 
Pour configurer le DNSSEC pour une zone: 
```bash
# Créer le dossier des clefs
mkdir /var/cache/bind/keys
chmod ug=rwx,o=,g+s /var/cache/bind/keys
chgrp bind /var/cache/bind/keys

# Créer les clefs
cd /var/cache/bind/keys

# La liste des algorithmes est disponible ici : https://www.iana.org/assignments/dns-sec-alg-numbers/dns-sec-alg-numbers.xhtml
algorithm=ECDSAP384SHA384
domain=<domain>
# Pour les algorithmes RSA préciser le nombre de bits avec -b
dnssec-keygen -a $algorithm -f KSK $domain
dnssec-keygen -a $algorithm $domain
chmod g+r *.private

systemctl reload bind9
rndc loadkeys $domain
rndc signing -nsec3param 1 0 10 auto $domain

# Obtenir les clefs
host -t DNSKEY $domain localhost
# La clef qui nous intéresse est la KSK (Flag 257)
# Il est possible d'obtenir son Tag en allant dans /var/cache/bind/keys ou en utilisant un outil comme https://dnsviz.net/

# Afficher des informations sur la zone
rndc zonestatus $domain
```