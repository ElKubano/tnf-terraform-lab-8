# CREATION DE LA BASE DE DONNEES MYSQL

Comme expliqué en introduction, l’infrastructure que vous devez créer comprend une base de données MySQL dont la fonction est de stocker les données soumises depuis la page web de l’application (qui n’est pas encore installée sur votre serveur).

Cette base de données sera créée au travers du service [RDS (Relational Database Service)](https://aws.amazon.com/fr/rds/?nc1=h_ls) d’AWS.

## Déclaration de la base de données

En vous appuyant sur la [documentation officielle de terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance), déclarez dans un fichier **database.tf** la base de données MySQL avec les caractéristiques suivantes :

| Caractéristique                         | Valeur                                                                                            |
|-----------------------------------------|---------------------------------------------------------------------------------------------------|
| Nom                                     | nuumfactory-\<environnement\>-db-\<digit\> (Utilisez la variable locale correspondante)           |
| Stockage alloué                         | 10Go                                                                                              |
| Nom de l’instance de base de données    | nuumfactorydb\<digit\>                                                                            |
| Moteur                                  | MySQL                                                                                             |
| Version du moteur                       | 8.0                                                                                               |
| Sizing                                  | db.t3.micro                                                                                       |
| Nom du subnet group associé             | "nuumfactory-db-subnet-group"                                                                     |
| Security group                          | Votre security group DB                                                                           |
| Nom de l’utilisateur principal          | dbadmin                                                                                           |
| Mot de passe de l’utilisateur principal | dbadminpassword                                                                                   |
| Snapshot final à la destruction ?       | Non                                                                                               |

*N.B : Renseigner des credentials (login / mot de passe) dans votre code terraform n’est pas du tout conseillé. Une bonne pratique est d’utiliser des outils tiers tels qu’Hashicorp Vault, AWS Secret Manager ou Conjur pour stocker ces informations sensibles.*

## Création de la base de données

Exécutez la commande **terraform fmt** puis la commande **terraform plan -var-file="developpement.tfvars"** : Corrigez les éventuelles erreurs obtenues et réexécutez la commande jusqu’à ne plus en obtenir.

Observez le plan d’exécution et s’il correspond à ce que vous souhaitez réaliser, exécutez la commande **terraform apply -auto-approve -var-file="developpement.tfvars"**. Terraform procède à la création de votre base de données MySQL.