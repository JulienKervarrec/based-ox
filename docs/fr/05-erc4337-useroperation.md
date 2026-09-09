# Chapitre 5 -- ERC-4337 : les types UserOperation multi-versions

`erc4337/UserOperation.ts` (943 lignes) et `erc4337/EntryPoint.ts` (plus de
2000 lignes) forment le plus gros module ERC du depot : ils modelisent la
structure de donnees centrale de l account abstraction ERC-4337, la
UserOperation, telle qu elle a evolue a travers trois versions successives
du standard EntryPoint (0.6, 0.7, 0.8).

Le type `UserOperation` est generique sur la version de l EntryPoint cible
(`entryPointVersion extends EntryPoint.Version`) et se resout, via l
utilitaire `OneOf`, vers l une des trois formes concretes `V06`, `V07` ou
`V08` -- chaque version ayant historiquement fait evoluer la structure des
champs (par exemple le regroupement des limites de gas ou des donnees de
paymaster). Un second type, `Packed`, modelise la representation
"compactee" de la UserOperation telle qu elle est effectivement envoyee
on-chain a l EntryPoint : plusieurs champs logiquement distincts y sont
concatenes dans un seul mot de 32 octets pour economiser du gas, par
exemple `accountGasLimits` qui concatene `verificationGasLimit` (16 octets)
et `callGasLimit` (16 octets), ou `gasFees` qui concatene
`maxPriorityFee` et `maxFeePerGas`.

Cette double representation (structure "deballee" lisible cote application,
et structure "compactee" au format on-chain) est un motif recurrent de
l account abstraction : le module encapsule la logique de packing/unpacking
pour que le code appelant manipule des champs nommes plutot que des
concatenations d octets, tout en produisant en sortie exactement le format
attendu par le contrat EntryPoint deploye sur la chaine.
