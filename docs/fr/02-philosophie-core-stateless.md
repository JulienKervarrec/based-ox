# Chapitre 2 -- Le module core : primitives sans etat, verbeuses par choix

Le README illustre la philosophie de conception d ox avec un exemple
complet de construction, signature et diffusion d une transaction EIP-1559,
en cinq etapes explicites et separees : construire l enveloppe via
`TransactionEnvelopeEip1559.from(...)`, obtenir le payload a signer via
`getSignPayload`, signer ce payload avec `Secp256k1.sign` en fournissant la
cle privee, serialiser l enveloppe avec sa signature via `serialize`, puis
diffuser le resultat serialise via un `Provider` (`window.ethereum`) avec la
methode RPC `eth_sendRawTransaction`.

Le texte precise explicitement que cette verbosite est voulue : les memes
cinq etapes pourraient etre condensees en quelques lignes par une
abstraction de plus haut niveau, mais l objectif de ox est justement de
laisser cette condensation aux bibliotheques construites par-dessus (Viem
en tete) plutot que de l imposer lui-meme. Cette contrainte de conception
se retrouve dans la structure meme du module `core/` : chaque primitive
(`Hex`, `Address`, `AbiParameters`, `Secp256k1`, `Signature`, ...) est un
module independant, sans instance ni etat partage entre appels -- une
fonction comme `Secp256k1.sign` prend explicitement `payload` et
`privateKey` en parametres plutot que de dependre d un objet client
configure au prealable.

Ce choix architectural explique aussi pourquoi les modules specifiques a un
ERC (chapitres 3 a 5) sont geographiquement separes du `core/` : ils
importent les primitives de base (`Hex`, `AbiParameters`, `Secp256k1`,
`Signature`) plutot que de dupliquer leur propre logique de bas niveau,
gardant chaque standard comme une couche fine au-dessus d une fondation
commune.
