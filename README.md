#### TPNGS

### Premiere étape = Mapping (on aligne les séquences des individus au chromosome de réference)
## Récupération de la séquence du chromosome 20 du génome de réference à prtir d'une base de donnée (fonction wget) et indexation (fct bwa index)
## Fille:
# Récupération de la séquence du chromosome 20 à partir de la base de données 1000 génomes (fonction wget)
# Alignement et application des reads au bon endroit de la séquence de référence (paired reads) (fonction bwa meme)
# Post-traitement et filtrage des reads non alignés (fonction samtools view)
# Tri des reads pour les mettre dans le bon ordre (fonction samtools sort)
# Pondération des alignements par les métadonnées (fonction gatk AddOrReplaceReadGroups)
# Indexation 
## Mère :
# Récupération de la séquence du chromosome 20 de la mère (fonction wget)
# Alignement, filtrage et tri en 1 seule ligne grace au "|" 
# Pondération des alignements par les métadonnées (fonction gatk AddOrReplaceReadGroups)
# Indexation 
## Père
# définition de varible dans l'URL que l'on récupère (contenant plusieurs alignementsà, ainsi que les informations sur le séquençage
# filtration des doublons dans les données (fonction "grep")
# boucle afin de construire un seul alignement pour le pere : 
#définition d'une liste, pour l'instant vide,
#pour les 8 premiers runs, algnement à la reference, filtrage, tri et ajout à la liste précédemment créée
# Assemblage des alignements (samtool merge)
# Indexation


### Deuxieme étape : Variant calling (repérage des variants sur les génomes étudiés)

## Préparation des données GATK
# Téléchargement des indels et SNP connus à partir des données 1000 génome (fonction wget)
# indexation du génome de référence (fonction samtools faidx) et création d'un dictionnaire 
# Retrait des duplicat et indexation des alignements de la fille, de la mere et du pere (fonction MarkDuplicates puis samtool index)

## Réalignement local autour des indels
# Repérage et réalignement des régions devant etre réalignées ( fonction gatk RealignerTargetCreator puis gatk IndelRealigner)

## Recalibraton selon la qualité du séquençage (Base Quality Score Recalibration = indice de confiance)
# Production d'une table de recalibration par modélisation empirique de l'erreur pour chaque base et obtention d'un pattern de covariance sur le dataset (fonction gatk BaseRecalibrator)
# Recalibration= correction des erreurs systématiques (fonction gatk PrintReads avec option BQSR)

## Variant calling
# Repérage des variants par comparaison avec le génome de référence, les SMP et indels connus (gatk HaplotypeCaller permettant d'obtenir un ficher .g.vcf à partir d'un .bam, puis gatk GenotypeGVCFs pour obtenir un fichier .vcf)


### Troisième étape = Trio-Analysis (permet de déterminer le mode d'héritage des variants)
# Calcule des combinaison les plus probabke selon les contraintes familiales ( fonction gatk PhaseByTransmission)
# Calcul ddes métriques de controle (fonction gatk VariantEval)
# détermination du niveau de concordance entre deux genomes (fichier .vcf) (focntion gatk GenotypeConcordance)
