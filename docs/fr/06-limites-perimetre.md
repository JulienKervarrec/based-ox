# Chapitre 6 -- Limites et perimetre de ce parcours

Ce parcours couvre la presentation generale du depot et sa relation avec
le projet upstream "ox", la philosophie de conception du module `core/`
(illustree par l exemple de transaction du README), les modules ERC-6492 et
ERC-8010 pour les signatures enveloppees, le module ERC-7821 pour l execution
groupee d appels, et le module ERC-4337 pour les types UserOperation
multi-versions.

Sont volontairement laisses hors champ : le detail exhaustif des dizaines
de primitives du module `core/` (`Hex`, `Address`, `AbiParameters`, `RLP`,
`TypedData`, `Kzg`, et bien d autres, chacune avec sa propre API complete)
au-dela de leur role illustratif au chapitre 2 ; le detail complet du module
`erc4337/EntryPoint.ts` (plus de 2000 lignes, couvrant vraisemblablement
l ABI complet des trois versions d EntryPoint et leurs fonctions de
simulation/validation) au-dela de sa mention au chapitre 5 ; les contrats
Solidity de `contracts/src/` (le validateur de signature universel et sa
variante deployless) au-dela de leur bytecode mentionne au chapitre 3 ; les
"trusted setups" KZG utilises pour les blobs EIP-4844 ; et l outillage de
build, de documentation (Vocs, docgen) et de publication npm/JSR du depot.

L objectif reste le meme que pour les parcours precedents de cette
bibliotheque : comprendre precisement comment cette bibliotheque bas niveau
structure les primitives Ethereum et leurs extensions specifiques a l
account abstraction, sans pretendre couvrir l integralite d une
bibliotheque standard aussi vaste.
